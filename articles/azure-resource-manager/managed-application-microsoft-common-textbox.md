---
title: "aaaAzure spravované aplikace textového elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.TextBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 11771cd1d689b720384df98b8d1465703068af37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommontextbox-ui-element"></a><span data-ttu-id="94416-103">Element Microsoft.Common.TextBox uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="94416-103">Microsoft.Common.TextBox UI element</span></span>
<span data-ttu-id="94416-104">Ovládací prvek, který lze použít tooedit neformátovaný text.</span><span class="sxs-lookup"><span data-stu-id="94416-104">A control that can be used tooedit unformatted text.</span></span> <span data-ttu-id="94416-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="94416-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="94416-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="94416-106">UI sample</span></span>
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a><span data-ttu-id="94416-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="94416-108">Schema</span></span>
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
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="94416-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="94416-109">Remarks</span></span>
- <span data-ttu-id="94416-110">Pokud `constraints.required` je nastaven příliš**true**, pak hello textového pole musí obsahovat hodnotu toovalidate úspěšně.</span><span class="sxs-lookup"><span data-stu-id="94416-110">If `constraints.required` is set too**true**, then hello text box must contain a value toovalidate successfully.</span></span> <span data-ttu-id="94416-111">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="94416-111">hello default value is **false**.</span></span>
- <span data-ttu-id="94416-112">`constraints.regex`je vzor regulárního výrazu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="94416-112">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="94416-113">-Li zadána, pak hello textového pole Hodnota musí odpovídat hello vzor toovalidate úspěšně.</span><span class="sxs-lookup"><span data-stu-id="94416-113">If specified, then hello text box's value must match hello pattern toovalidate successfully.</span></span> <span data-ttu-id="94416-114">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="94416-114">The default value is **null**.</span></span>
- <span data-ttu-id="94416-115">`constraints.validationMessage`je řetězec toodisplay při ověřování se nezdaří, hello jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="94416-115">`constraints.validationMessage` is a string toodisplay when hello text box's value fails validation.</span></span> <span data-ttu-id="94416-116">Pokud není zadaný, bude hello textového pole integrované ověřování, které se používají zprávy.</span><span class="sxs-lookup"><span data-stu-id="94416-116">If not specified, then hello text box's built-in validation messages are used.</span></span> <span data-ttu-id="94416-117">Hello výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="94416-117">hello default value is **null**.</span></span>
- <span data-ttu-id="94416-118">Jeho možných toospecify hodnota pro `constraints.regex` při `constraints.required` je nastaven příliš**false**.</span><span class="sxs-lookup"><span data-stu-id="94416-118">It's possible toospecify a value for `constraints.regex` when `constraints.required` is set too**false**.</span></span> <span data-ttu-id="94416-119">V tomto scénáři hodnota není potřeba hello textové pole toovalidate úspěšně.</span><span class="sxs-lookup"><span data-stu-id="94416-119">In this scenario, a value is not required for hello text box toovalidate successfully.</span></span> <span data-ttu-id="94416-120">Pokud je zadaná, musí se shodovat hello vzor regulárního výrazu.</span><span class="sxs-lookup"><span data-stu-id="94416-120">If one is specified, it must match hello regular expression pattern.</span></span>

## <a name="sample-output"></a><span data-ttu-id="94416-121">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="94416-121">Sample output</span></span>

```json
"foobar"
```

## <a name="next-steps"></a><span data-ttu-id="94416-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94416-122">Next steps</span></span>
* <span data-ttu-id="94416-123">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="94416-123">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="94416-124">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="94416-124">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="94416-125">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="94416-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
