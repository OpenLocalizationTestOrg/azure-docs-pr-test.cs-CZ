---
title: "element uživatelského rozhraní Položka PasswordBox spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.PasswordBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: bcb1f54c0bee464075ed732ead9aa3f88697f49e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonpasswordbox-ui-element"></a><span data-ttu-id="b0f6a-103">Element Microsoft.Common.PasswordBox uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="b0f6a-103">Microsoft.Common.PasswordBox UI element</span></span>
<span data-ttu-id="b0f6a-104">Ovládací prvek, který lze použít tooprovide a potvrzení hesla.</span><span class="sxs-lookup"><span data-stu-id="b0f6a-104">A control that can be used tooprovide and confirm a password.</span></span> <span data-ttu-id="b0f6a-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="b0f6a-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="b0f6a-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="b0f6a-106">UI sample</span></span>
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a><span data-ttu-id="b0f6a-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="b0f6a-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="b0f6a-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b0f6a-109">Remarks</span></span>
- <span data-ttu-id="b0f6a-110">Tento element nepodporuje hello `defaultValue` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b0f6a-110">This element doesn't support hello `defaultValue` property.</span></span>
- <span data-ttu-id="b0f6a-111">Podrobnosti implementace `constraints`, najdete v části [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span><span class="sxs-lookup"><span data-stu-id="b0f6a-111">For implementation details of `constraints`, see [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md).</span></span>
- <span data-ttu-id="b0f6a-112">Pokud `options.hideConfirmation` je nastaven příliš**true**, hello druhé textové pole pro potvrzení hesla hello uživatele je skrytá.</span><span class="sxs-lookup"><span data-stu-id="b0f6a-112">If `options.hideConfirmation` is set too**true**, hello second text box for confirming hello user's password is hidden.</span></span> <span data-ttu-id="b0f6a-113">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="b0f6a-113">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="b0f6a-114">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="b0f6a-114">Sample output</span></span>
```json
"p4ssw0rd"
```

## <a name="next-steps"></a><span data-ttu-id="b0f6a-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0f6a-115">Next steps</span></span>
* <span data-ttu-id="b0f6a-116">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b0f6a-116">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="b0f6a-117">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b0f6a-117">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="b0f6a-118">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="b0f6a-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
