---
title: "aaaAzure spravované aplikace odesílání souborů při odpovědích elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Common.FileUpload uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 7af5bec992e3f120afb1bdf56d8b4c19a8e5e834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonfileupload-ui-element"></a>Element Microsoft.Common.FileUpload uživatelského rozhraní
Ovládací prvek, který umožňuje toospecify uživatele jeden nebo více souborů tooupload. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Common.FileUpload",
  "label": "Some file upload",
  "toolTip": "",
  "constraints": {
    "required": true,
    "accept": ".doc,.docx,.xml,application/msword"
  },
  "options": {
    "multiple": false,
    "uploadMode": "file",
    "openMode": "text",
    "encoding": "UTF-8"
  },
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- `constraints.accept`Určuje hello typy souborů, které se zobrazují v dialogovém okně prohlížeče hello souboru. V tématu hello [specifikace HTML5](http://www.w3.org/TR/html5/forms.html#attr-input-accept) pro povolené hodnoty. Hello výchozí hodnota je **null**.
- Pokud `options.multiple` je nastaven příliš**true**, hello uživatel tooselect více než jeden soubor v dialogovém okně prohlížeče hello souboru. Hello výchozí hodnota je **false**.
- Tento element podporuje odesílání souborů ve dvou režimech na základě hodnoty hello `options.uploadMode`. Pokud **soubor** není zadaný, výstup hello obsahuje hello obsah souboru hello jako objekt blob. Pokud **url** je určeno, soubor hello nahrané tooa dočasného umístění a výstup hello obsahuje hello URL objektu hello blob. Dočasné objekty BLOB se vyprázdní po 24 hodinách. Hello výchozí hodnota je **soubor**.
- Hello hodnota `options.openMode` Určuje, jak je hello souboru pro čtení. Pokud soubor hello očekávané toobe prostý text, zadejte **text**; v opačném, zadejte **binární**. Hello výchozí hodnota je **text**.
- Pokud `options.uploadMode` je nastaven příliš**soubor** a `options.openMode` je nastaven příliš**binární**, výstup hello se kódování base64.
- `options.encoding`Určuje kódování toouse hello při čtení souboru hello. Hello výchozí hodnota je **UTF-8**a používá se pouze tehdy, když `options.openMode` je nastaven příliš**text**.

## <a name="sample-output"></a>Ukázkový výstup
Pokud je hodnota false options.multiple a options.uploadMode je soubor, výstup obsahuje hello obsah souboru hello jako řetězec formátu JSON:

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

Pokud platí options.multiple and'options.uploadMode je soubor, pak výstup obsahuje hello obsah souborů hello jako pole JSON:

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

Pokud je hodnota false options.multiple a options.uploadMode je adresa url, výstup obsahuje adresu URL jako řetězec formátu JSON:

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

Pokud je true options.multiple a options.uploadMode je adresa url, výstup obsahuje seznam adres URL jako pole JSON:
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

Při testování CreateUiDefinition, zkrátit některé prohlížeče (například Google Chrome) adresy URL vygenerovaných elementem hello Microsoft.Common.FileUpload v konzole prohlížeče hello. Může být nutné tooright kliknutím jednotlivé odkazy toocopy hello úplné adresy URL.


## <a name="next-steps"></a>Další kroky
* Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).
* Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).
