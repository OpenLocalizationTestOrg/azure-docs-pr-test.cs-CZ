---
title: "Azure spravované aplikace textového elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Common.TextBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 5a0ac5b811812c8c03f7f63aae12b8699d248ebf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommontextbox-ui-element"></a><span data-ttu-id="9ac78-103">Element Microsoft.Common.TextBox uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="9ac78-103">Microsoft.Common.TextBox UI element</span></span>
<span data-ttu-id="9ac78-104">Ovládací prvek, který lze upravit neformátovaný text.</span><span class="sxs-lookup"><span data-stu-id="9ac78-104">A control that can be used to edit unformatted text.</span></span> <span data-ttu-id="9ac78-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="9ac78-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="9ac78-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="9ac78-106">UI sample</span></span>
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a><span data-ttu-id="9ac78-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="9ac78-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Halp!",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="9ac78-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9ac78-109">Remarks</span></span>
- <span data-ttu-id="9ac78-110">Pokud `constraints.required` je nastaven na **true**, pak textového pole musí obsahovat hodnotu úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="9ac78-110">If `constraints.required` is set to **true**, then the text box must contain a value to validate successfully.</span></span> <span data-ttu-id="9ac78-111">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="9ac78-111">The default value is **false**.</span></span>
- <span data-ttu-id="9ac78-112">`constraints.regex`je vzor regulárního výrazu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9ac78-112">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="9ac78-113">-Li zadána, hodnota textového pole musí odpovídat vzorku úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="9ac78-113">If specified, then the text box's value must match the pattern to validate successfully.</span></span> <span data-ttu-id="9ac78-114">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="9ac78-114">The default value is **null**.</span></span>
- <span data-ttu-id="9ac78-115">`constraints.validationMessage`je řetězec k zobrazení při ověřování se nezdaří, jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9ac78-115">`constraints.validationMessage` is a string to display when the text box's value fails validation.</span></span> <span data-ttu-id="9ac78-116">Pokud není zadaný, se používají zpráv integrované ověření textového pole.</span><span class="sxs-lookup"><span data-stu-id="9ac78-116">If not specified, then the text box's built-in validation messages are used.</span></span> <span data-ttu-id="9ac78-117">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="9ac78-117">The default value is **null**.</span></span>
- <span data-ttu-id="9ac78-118">Je možné zadat hodnotu pro `constraints.regex` při `constraints.required` je nastaven na **false**.</span><span class="sxs-lookup"><span data-stu-id="9ac78-118">It's possible to specify a value for `constraints.regex` when `constraints.required` is set to **false**.</span></span> <span data-ttu-id="9ac78-119">V tomto scénáři se hodnota nevyžaduje pro textové pole úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="9ac78-119">In this scenario, a value is not required for the text box to validate successfully.</span></span> <span data-ttu-id="9ac78-120">Pokud je zadaná, musí se shodovat regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="9ac78-120">If one is specified, it must match the regular expression pattern.</span></span>

## <a name="sample-output"></a><span data-ttu-id="9ac78-121">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="9ac78-121">Sample output</span></span>

```json
"foobar"
```

## <a name="next-steps"></a><span data-ttu-id="9ac78-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ac78-122">Next steps</span></span>
* <span data-ttu-id="9ac78-123">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9ac78-123">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="9ac78-124">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9ac78-124">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="9ac78-125">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="9ac78-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
