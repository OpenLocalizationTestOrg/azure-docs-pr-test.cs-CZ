---
title: "Azure spravované aplikace Položka PasswordBox elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Common.PasswordBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 196a4b8f77145f83e46b4b23e148bb3a9dffc1b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonpasswordbox-ui-element"></a><span data-ttu-id="d4a2f-103">Element Microsoft.Common.PasswordBox uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d4a2f-103">Microsoft.Common.PasswordBox UI element</span></span>
<span data-ttu-id="d4a2f-104">Ovládací prvek, který slouží k zadání a potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="d4a2f-104">A control that can be used to provide and confirm a password.</span></span> <span data-ttu-id="d4a2f-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d4a2f-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="d4a2f-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d4a2f-106">UI sample</span></span>
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a><span data-ttu-id="d4a2f-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="d4a2f-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "",
    "validationMessage": ""
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="d4a2f-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d4a2f-109">Remarks</span></span>
- <span data-ttu-id="d4a2f-110">Tento element nepodporuje `defaultValue` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d4a2f-110">This element doesn't support the `defaultValue` property.</span></span>
- <span data-ttu-id="d4a2f-111">Podrobnosti implementace `constraints`, najdete v části [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span><span class="sxs-lookup"><span data-stu-id="d4a2f-111">For implementation details of `constraints`, see [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span></span>
- <span data-ttu-id="d4a2f-112">Pokud `options.hideConfirmation` je nastaven na **true**, druhé textové pole pro potvrzení hesla je skrytá.</span><span class="sxs-lookup"><span data-stu-id="d4a2f-112">If `options.hideConfirmation` is set to **true**, the second text box for confirming the user's password is hidden.</span></span> <span data-ttu-id="d4a2f-113">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="d4a2f-113">The default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="d4a2f-114">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="d4a2f-114">Sample output</span></span>
```json
"p4ssw0rd"
```

## <a name="next-steps"></a><span data-ttu-id="d4a2f-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d4a2f-115">Next steps</span></span>
* <span data-ttu-id="d4a2f-116">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4a2f-116">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="d4a2f-117">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4a2f-117">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="d4a2f-118">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="d4a2f-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
