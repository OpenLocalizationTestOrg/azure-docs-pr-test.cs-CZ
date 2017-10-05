---
title: "Azure spravované aplikace UserNameTextBox elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Compute.UserNameTextBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: c90be5a0ed3aadda81d7ec9b5388a96472f69af0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a><span data-ttu-id="ad07a-103">Element Microsoft.Compute.UserNameTextBox uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ad07a-103">Microsoft.Compute.UserNameTextBox UI element</span></span>
<span data-ttu-id="ad07a-104">Ovládací prvek textové pole s integrované ověřování systému Windows a Linux uživatelská jména.</span><span class="sxs-lookup"><span data-stu-id="ad07a-104">A text box control with built-in validation for Windows and Linux user names.</span></span> <span data-ttu-id="ad07a-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="ad07a-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="ad07a-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ad07a-106">UI sample</span></span>
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a><span data-ttu-id="ad07a-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="ad07a-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="ad07a-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ad07a-109">Remarks</span></span>
- <span data-ttu-id="ad07a-110">Pokud `constraints.required` je nastaven na **true**, pak textového pole musí obsahovat hodnotu úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="ad07a-110">If `constraints.required` is set to **true**, then the text box must contain a value to validate successfully.</span></span> <span data-ttu-id="ad07a-111">Výchozí hodnota je **true**.</span><span class="sxs-lookup"><span data-stu-id="ad07a-111">The default value is **true**.</span></span>
- <span data-ttu-id="ad07a-112">`osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="ad07a-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="ad07a-113">`constraints.regex`je vzor regulárního výrazu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ad07a-113">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="ad07a-114">-Li zadána, hodnota textového pole musí odpovídat vzorku úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="ad07a-114">If specified, then the text box's value must match the pattern to validate successfully.</span></span> <span data-ttu-id="ad07a-115">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="ad07a-115">The default value is **null**.</span></span>
- <span data-ttu-id="ad07a-116">`constraints.validationMessage`je řetězec k zobrazení při jeho hodnotu neprojde ověřením určeného `constraints.regex`.</span><span class="sxs-lookup"><span data-stu-id="ad07a-116">`constraints.validationMessage` is a string to display when the text box's value fails the validation specified by `constraints.regex`.</span></span> <span data-ttu-id="ad07a-117">Pokud není zadaný, se používají zpráv integrované ověření textového pole.</span><span class="sxs-lookup"><span data-stu-id="ad07a-117">If not specified, then the text box's built-in validation messages are used.</span></span> <span data-ttu-id="ad07a-118">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="ad07a-118">The default value is **null**.</span></span>
- <span data-ttu-id="ad07a-119">Tento element má integrované ověřování, který je založen na hodnotu zadanou pro `osPlatform`.</span><span class="sxs-lookup"><span data-stu-id="ad07a-119">This element has built-in validation that is based on the value specified for `osPlatform`.</span></span> <span data-ttu-id="ad07a-120">Integrované ověřování můžete použít vlastní regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="ad07a-120">The built-in validation can be used along with a custom regular expression.</span></span>
<span data-ttu-id="ad07a-121">Pokud nezadáte hodnotu `constraints.regex` je určeno, aktivaci předdefinované a vlastní ověření.</span><span class="sxs-lookup"><span data-stu-id="ad07a-121">If a value for `constraints.regex` is specified, then both the built-in and custom validations are triggered.</span></span>

## <a name="sample-output"></a><span data-ttu-id="ad07a-122">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="ad07a-122">Sample output</span></span>
```json
"tabrezm"
```

## <a name="next-steps"></a><span data-ttu-id="ad07a-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad07a-123">Next steps</span></span>
* <span data-ttu-id="ad07a-124">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad07a-124">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="ad07a-125">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad07a-125">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="ad07a-126">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="ad07a-126">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
