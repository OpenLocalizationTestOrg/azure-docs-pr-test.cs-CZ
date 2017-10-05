---
title: "Azure spravované aplikace CredentialsCombo elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Compute.CredentialsCombo uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 254f383ee6f7cb9f7051fa135d85319a22c3c369
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a><span data-ttu-id="d9862-103">Element Microsoft.Compute.CredentialsCombo uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d9862-103">Microsoft.Compute.CredentialsCombo UI element</span></span>
<span data-ttu-id="d9862-104">Skupina ovládacích prvků pomocí integrované ověřování systému Windows a Linux hesla a veřejného klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="d9862-104">A group of controls with built-in validation for Windows and Linux passwords and SSH public keys.</span></span> <span data-ttu-id="d9862-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d9862-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="d9862-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d9862-106">UI sample</span></span>
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a><span data-ttu-id="d9862-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="d9862-108">Schema</span></span>
<span data-ttu-id="d9862-109">Pokud `osPlatform` je **Windows**, je použita na následující schéma:</span><span class="sxs-lookup"><span data-stu-id="d9862-109">If `osPlatform` is **Windows**, then the following schema is used:</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

<span data-ttu-id="d9862-110">Pokud `osPlatform` je **Linux**, je použita na následující schéma:</span><span class="sxs-lookup"><span data-stu-id="d9862-110">If `osPlatform` is **Linux**, then the following schema is used:</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="d9862-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d9862-111">Remarks</span></span>
- <span data-ttu-id="d9862-112">`osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="d9862-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="d9862-113">Pokud `constraints.required` je nastaven na **true**, pak heslo nebo SSH veřejného klíče textová pole musí obsahovat hodnoty úspěšně ověřit.</span><span class="sxs-lookup"><span data-stu-id="d9862-113">If `constraints.required` is set to **true**, then the password or SSH public key text boxes must contain values to validate successfully.</span></span> <span data-ttu-id="d9862-114">Výchozí hodnota je **true**.</span><span class="sxs-lookup"><span data-stu-id="d9862-114">The default value is **true**.</span></span>
- <span data-ttu-id="d9862-115">Pokud `options.hideConfirmation` je nastaven na **true**, druhé textové pole pro potvrzení hesla je skrytý.</span><span class="sxs-lookup"><span data-stu-id="d9862-115">If `options.hideConfirmation` is set to **true**, then the second text box for confirming the user's password is hidden.</span></span> <span data-ttu-id="d9862-116">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="d9862-116">The default value is **false**.</span></span>
- <span data-ttu-id="d9862-117">Pokud `options.hidePassword` je nastaven na **true**, možnost použít ověřování hesla je skrytý.</span><span class="sxs-lookup"><span data-stu-id="d9862-117">If `options.hidePassword` is set to **true**, then the option to use password authentication is hidden.</span></span> <span data-ttu-id="d9862-118">Lze jej použít pouze tehdy, když `osPlatform` je **Linux**.</span><span class="sxs-lookup"><span data-stu-id="d9862-118">It can be used only when `osPlatform` is **Linux**.</span></span> <span data-ttu-id="d9862-119">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="d9862-119">The default value is **false**.</span></span>
- <span data-ttu-id="d9862-120">Další omezení povolených hesel můžete implementovat pomocí `customPasswordRegex` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d9862-120">Additional constraints on the allowed passwords can be implemented by using the `customPasswordRegex` property.</span></span> <span data-ttu-id="d9862-121">Řetězec v `customValidationMessage` se zobrazí, pokud heslo vlastní ověřování se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="d9862-121">The string in `customValidationMessage` is displayed when a password fails custom validation.</span></span> <span data-ttu-id="d9862-122">Výchozí hodnota pro obě vlastnosti je **null**.</span><span class="sxs-lookup"><span data-stu-id="d9862-122">The default value for both properties is **null**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="d9862-123">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="d9862-123">Sample output</span></span>
<span data-ttu-id="d9862-124">Pokud `osPlatform` je **Windows**, nebo uživatele zadali heslo místo veřejný klíč SSH a pak se očekává následující výstup:</span><span class="sxs-lookup"><span data-stu-id="d9862-124">If `osPlatform` is **Windows**, or the user provided a password instead of an SSH public key, then the following output is expected:</span></span>

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

<span data-ttu-id="d9862-125">Pokud uživatel zadaný veřejný klíč SSH, je očekáván následující výstup:</span><span class="sxs-lookup"><span data-stu-id="d9862-125">If the user provided an SSH public key, then the following output is expected:</span></span>
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a><span data-ttu-id="d9862-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9862-126">Next steps</span></span>
* <span data-ttu-id="d9862-127">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d9862-127">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="d9862-128">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d9862-128">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="d9862-129">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="d9862-129">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>