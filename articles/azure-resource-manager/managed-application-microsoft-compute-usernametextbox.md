---
title: "element uživatelského rozhraní UserNameTextBox spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Compute.UserNameTextBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 33092014e804c4aabd56ba49144d9cd4d6a5fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a><span data-ttu-id="3333e-103">Element Microsoft.Compute.UserNameTextBox uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3333e-103">Microsoft.Compute.UserNameTextBox UI element</span></span>
<span data-ttu-id="3333e-104">Ovládací prvek textové pole s integrované ověřování systému Windows a Linux uživatelská jména.</span><span class="sxs-lookup"><span data-stu-id="3333e-104">A text box control with built-in validation for Windows and Linux user names.</span></span> <span data-ttu-id="3333e-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3333e-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="3333e-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3333e-106">UI sample</span></span>
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a><span data-ttu-id="3333e-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="3333e-108">Schema</span></span>
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
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="3333e-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3333e-109">Remarks</span></span>
- <span data-ttu-id="3333e-110">Pokud `constraints.required` je nastaven příliš**true**, pak hello textového pole musí obsahovat hodnotu toovalidate úspěšně.</span><span class="sxs-lookup"><span data-stu-id="3333e-110">If `constraints.required` is set too**true**, then hello text box must contain a value toovalidate successfully.</span></span> <span data-ttu-id="3333e-111">Hello výchozí hodnota je **true**.</span><span class="sxs-lookup"><span data-stu-id="3333e-111">hello default value is **true**.</span></span>
- <span data-ttu-id="3333e-112">`osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="3333e-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="3333e-113">`constraints.regex`je vzor regulárního výrazu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3333e-113">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="3333e-114">-Li zadána, pak hello textového pole Hodnota musí odpovídat hello vzor toovalidate úspěšně.</span><span class="sxs-lookup"><span data-stu-id="3333e-114">If specified, then hello text box's value must match hello pattern toovalidate successfully.</span></span> <span data-ttu-id="3333e-115">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="3333e-115">The default value is **null**.</span></span>
- <span data-ttu-id="3333e-116">`constraints.validationMessage`je toodisplay řetězec, pokud se nezdaří ověření hello určeného hello jeho hodnotu `constraints.regex`.</span><span class="sxs-lookup"><span data-stu-id="3333e-116">`constraints.validationMessage` is a string toodisplay when hello text box's value fails hello validation specified by `constraints.regex`.</span></span> <span data-ttu-id="3333e-117">Pokud není zadaný, bude hello textového pole integrované ověřování, které se používají zprávy.</span><span class="sxs-lookup"><span data-stu-id="3333e-117">If not specified, then hello text box's built-in validation messages are used.</span></span> <span data-ttu-id="3333e-118">Hello výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="3333e-118">hello default value is **null**.</span></span>
- <span data-ttu-id="3333e-119">Tento element má integrované ověřování, který je založen na hello hodnota zadaná pro `osPlatform`.</span><span class="sxs-lookup"><span data-stu-id="3333e-119">This element has built-in validation that is based on hello value specified for `osPlatform`.</span></span> <span data-ttu-id="3333e-120">Hello integrované ověřování můžete použít vlastní regulární výraz.</span><span class="sxs-lookup"><span data-stu-id="3333e-120">hello built-in validation can be used along with a custom regular expression.</span></span>
<span data-ttu-id="3333e-121">Pokud nezadáte hodnotu `constraints.regex` je zadán, pak oba hello předdefinované a vlastní ověření se aktivují.</span><span class="sxs-lookup"><span data-stu-id="3333e-121">If a value for `constraints.regex` is specified, then both hello built-in and custom validations are triggered.</span></span>

## <a name="sample-output"></a><span data-ttu-id="3333e-122">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="3333e-122">Sample output</span></span>
```json
"tabrezm"
```

## <a name="next-steps"></a><span data-ttu-id="3333e-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3333e-123">Next steps</span></span>
* <span data-ttu-id="3333e-124">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3333e-124">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="3333e-125">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3333e-125">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="3333e-126">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="3333e-126">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
