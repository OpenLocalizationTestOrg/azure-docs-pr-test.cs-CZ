---
title: "aaaAzure spravované aplikace část elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.Section uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: d20b365b12fab66177e1a12db2ebbeefe507212e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonsection-ui-element"></a><span data-ttu-id="acb1b-103">Element Microsoft.Common.Section uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="acb1b-103">Microsoft.Common.Section UI element</span></span>
<span data-ttu-id="acb1b-104">Ovládací prvek, který seskupuje jeden či více elementů pod nadpisem.</span><span class="sxs-lookup"><span data-stu-id="acb1b-104">A control that groups one or more elements under a heading.</span></span> <span data-ttu-id="acb1b-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="acb1b-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="acb1b-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="acb1b-106">UI sample</span></span>
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a><span data-ttu-id="acb1b-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="acb1b-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="acb1b-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="acb1b-109">Remarks</span></span>
- <span data-ttu-id="acb1b-110">`elements`musí obsahovat alespoň jeden element a může obsahovat všechny typy element kromě `Microsoft.Common.Section`.</span><span class="sxs-lookup"><span data-stu-id="acb1b-110">`elements` must contain at least one element, and can contain all element types except `Microsoft.Common.Section`.</span></span>
- <span data-ttu-id="acb1b-111">Tento element nepodporuje hello `toolTip` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="acb1b-111">This element doesn't support hello `toolTip` property.</span></span>

## <a name="sample-output"></a><span data-ttu-id="acb1b-112">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="acb1b-112">Sample output</span></span>
<span data-ttu-id="acb1b-113">hodnoty elementů v výstup tooaccess hello `elements`, použijte hello [basics()](managed-application-createuidefinition-functions.md#basics) nebo [steps()](managed-application-createuidefinition-functions.md#steps) funkce a s tečkami:</span><span class="sxs-lookup"><span data-stu-id="acb1b-113">tooaccess hello output values of elements in `elements`, use hello [basics()](managed-application-createuidefinition-functions.md#basics) or [steps()](managed-application-createuidefinition-functions.md#steps) functions and dot notation:</span></span>

```json
basics('section1').element1
```

<span data-ttu-id="acb1b-114">Elementy typu `Microsoft.Common.Section` mít žádné hodnoty výstup, sami.</span><span class="sxs-lookup"><span data-stu-id="acb1b-114">Elements of type `Microsoft.Common.Section` have no output values themselves.</span></span>

## <a name="next-steps"></a><span data-ttu-id="acb1b-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acb1b-115">Next steps</span></span>
* <span data-ttu-id="acb1b-116">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="acb1b-116">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="acb1b-117">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="acb1b-117">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="acb1b-118">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="acb1b-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
