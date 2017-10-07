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
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Element Microsoft.Compute.CredentialsCombo uživatelského rozhraní
Skupina ovládacích prvků pomocí integrované ověřování systému Windows a Linux hesla a veřejného klíče SSH. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a>Schéma
Pokud `osPlatform` je **Windows**, hello následující schéma bude použit:
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

Pokud `osPlatform` je **Linux**, hello následující schéma bude použit:
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

## <a name="remarks"></a>Poznámky
- `osPlatform`musí být zadán, a může být buď **Windows** nebo **Linux**.
- Pokud `constraints.required` je nastaven příliš**true**, pak hello heslo nebo SSH veřejného klíče textová pole musí obsahovat hodnoty toovalidate úspěšně. Hello výchozí hodnota je **true**.
- Pokud `options.hideConfirmation` je nastaven příliš**true**, hello druhé textové pole pro potvrzení hesla hello uživatele je skrytý. Hello výchozí hodnota je **false**.
- Pokud `options.hidePassword` je nastaven příliš**true**, ověřování hesla toouse možnost hello je skrytý. Lze jej použít pouze tehdy, když `osPlatform` je **Linux**. Výchozí hodnota je **false**.
- Další omezení hello povolené hesel můžete implementovat pomocí hello `customPasswordRegex` vlastnost. Hello řetězec v `customValidationMessage` se zobrazí, pokud heslo vlastní ověřování se nezdaří. Hello výchozí hodnota pro obě vlastnosti je **null**.

## <a name="sample-output"></a>Ukázkový výstup
Pokud `osPlatform` je **Windows**, nebo hello uživatele zadali heslo místo veřejný klíč SSH a pak hello následující výstup očekává se:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

Pokud zadaná uživatelem hello veřejný klíč SSH, pak hello následující výstup se předpokládá, že:
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
