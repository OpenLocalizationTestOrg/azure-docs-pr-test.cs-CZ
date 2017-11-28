---
title: "element uživatelského rozhraní CredentialsCombo spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Compute.CredentialsCombo uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: d44a3929ebb7a5ff78b72f9eaeb6e52b098e266f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a><span data-ttu-id="066b9-103">Element Microsoft.Compute.CredentialsCombo uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="066b9-103">Microsoft.Compute.CredentialsCombo UI element</span></span>
<span data-ttu-id="066b9-104">Skupina ovládacích prvků pomocí integrované ověřování systému Windows a Linux hesla a veřejného klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="066b9-104">A group of controls with built-in validation for Windows and Linux passwords and SSH public keys.</span></span> <span data-ttu-id="066b9-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="066b9-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="066b9-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="066b9-106">UI sample</span></span>
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a><span data-ttu-id="066b9-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="066b9-108">Schema</span></span>
<span data-ttu-id="066b9-109">Pokud `osPlatform` je **Windows**, hello následující schéma bude použit:</span><span class="sxs-lookup"><span data-stu-id="066b9-109">If `osPlatform` is **Windows**, then hello following schema is used:</span></span>
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
    "customValidationMessage": "hello password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

<span data-ttu-id="066b9-110">Pokud `osPlatform` je **Linux**, hello následující schéma bude použit:</span><span class="sxs-lookup"><span data-stu-id="066b9-110">If `osPlatform` is **Linux**, then hello following schema is used:</span></span>
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
    "customValidationMessage": "hello password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="066b9-111">Poznámky</span><span class="sxs-lookup"><span data-stu-id="066b9-111">Remarks</span></span>
- <span data-ttu-id="066b9-112">`osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**.</span><span class="sxs-lookup"><span data-stu-id="066b9-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="066b9-113">Pokud `constraints.required` je nastaven příliš**true**, pak hello heslo nebo SSH veřejného klíče textová pole musí obsahovat hodnoty toovalidate úspěšně.</span><span class="sxs-lookup"><span data-stu-id="066b9-113">If `constraints.required` is set too**true**, then hello password or SSH public key text boxes must contain values toovalidate successfully.</span></span> <span data-ttu-id="066b9-114">Hello výchozí hodnota je **true**.</span><span class="sxs-lookup"><span data-stu-id="066b9-114">hello default value is **true**.</span></span>
- <span data-ttu-id="066b9-115">Pokud `options.hideConfirmation` je nastaven příliš**true**, hello druhé textové pole pro potvrzení hesla hello uživatele je skrytý.</span><span class="sxs-lookup"><span data-stu-id="066b9-115">If `options.hideConfirmation` is set too**true**, then hello second text box for confirming hello user's password is hidden.</span></span> <span data-ttu-id="066b9-116">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="066b9-116">hello default value is **false**.</span></span>
- <span data-ttu-id="066b9-117">Pokud `options.hidePassword` je nastaven příliš**true**, ověřování hesla toouse možnost hello je skrytý.</span><span class="sxs-lookup"><span data-stu-id="066b9-117">If `options.hidePassword` is set too**true**, then hello option toouse password authentication is hidden.</span></span> <span data-ttu-id="066b9-118">Lze jej použít pouze tehdy, když `osPlatform` je **Linux**.</span><span class="sxs-lookup"><span data-stu-id="066b9-118">It can be used only when `osPlatform` is **Linux**.</span></span> <span data-ttu-id="066b9-119">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="066b9-119">The default value is **false**.</span></span>
- <span data-ttu-id="066b9-120">Další omezení hello povolené hesel můžete implementovat pomocí hello `customPasswordRegex` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="066b9-120">Additional constraints on hello allowed passwords can be implemented by using hello `customPasswordRegex` property.</span></span> <span data-ttu-id="066b9-121">Hello řetězec v `customValidationMessage` se zobrazí, pokud heslo vlastní ověřování se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="066b9-121">hello string in `customValidationMessage` is displayed when a password fails custom validation.</span></span> <span data-ttu-id="066b9-122">Hello výchozí hodnota pro obě vlastnosti je **null**.</span><span class="sxs-lookup"><span data-stu-id="066b9-122">hello default value for both properties is **null**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="066b9-123">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="066b9-123">Sample output</span></span>
<span data-ttu-id="066b9-124">Pokud `osPlatform` je **Windows**, nebo hello uživatele zadali heslo místo veřejný klíč SSH a pak hello následující výstup očekává se:</span><span class="sxs-lookup"><span data-stu-id="066b9-124">If `osPlatform` is **Windows**, or hello user provided a password instead of an SSH public key, then hello following output is expected:</span></span>

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

<span data-ttu-id="066b9-125">Pokud zadaná uživatelem hello veřejný klíč SSH, pak hello následující výstup se předpokládá, že:</span><span class="sxs-lookup"><span data-stu-id="066b9-125">If hello user provided an SSH public key, then hello following output is expected:</span></span>
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a><span data-ttu-id="066b9-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="066b9-126">Next steps</span></span>
* <span data-ttu-id="066b9-127">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="066b9-127">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="066b9-128">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="066b9-128">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="066b9-129">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="066b9-129">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
