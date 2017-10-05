---
title: "Azure spravované aplikace odesílání souborů při odpovědích elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Common.FileUpload uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 217e9e63eb7cd198f70cee42b418867df9f1f993
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonfileupload-ui-element"></a><span data-ttu-id="3f9f6-103">Element Microsoft.Common.FileUpload uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3f9f6-103">Microsoft.Common.FileUpload UI element</span></span>
<span data-ttu-id="3f9f6-104">Ovládací prvek, který umožňuje uživateli zadat jeden nebo více souborů, které chcete nahrát.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-104">A control that allows a user to specify one or more files to upload.</span></span> <span data-ttu-id="3f9f6-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3f9f6-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="3f9f6-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="3f9f6-106">UI sample</span></span>
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a><span data-ttu-id="3f9f6-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="3f9f6-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="3f9f6-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3f9f6-109">Remarks</span></span>
- <span data-ttu-id="3f9f6-110">`constraints.accept`Určuje typy souborů, které se zobrazují v dialogovém okně prohlížeče souboru.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-110">`constraints.accept` specifies the types of files that are shown in the browser's file dialog.</span></span> <span data-ttu-id="3f9f6-111">Najdete v článku [specifikace HTML5](http://www.w3.org/TR/html5/forms.html#attr-input-accept) pro povolené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-111">See the [HTML5 specification](http://www.w3.org/TR/html5/forms.html#attr-input-accept) for allowed values.</span></span> <span data-ttu-id="3f9f6-112">Výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-112">The default value is **null**.</span></span>
- <span data-ttu-id="3f9f6-113">Pokud `options.multiple` je nastaven na **true**, uživatel může vybrat více než jeden soubor v dialogovém okně prohlížeče souboru.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-113">If `options.multiple` is set to **true**, the user is allowed to select more than one file in the browser's file dialog.</span></span> <span data-ttu-id="3f9f6-114">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-114">The default value is **false**.</span></span>
- <span data-ttu-id="3f9f6-115">Tento element podporuje odesílání souborů ve dvou režimech na základě hodnoty z `options.uploadMode`.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-115">This element supports uploading files in two modes based on the value of `options.uploadMode`.</span></span> <span data-ttu-id="3f9f6-116">Pokud **soubor** není zadaný, výstup obsahuje obsah souboru jako objekt blob.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-116">If **file** is specified, the output contains the contents of the file as a blob.</span></span> <span data-ttu-id="3f9f6-117">Pokud **url** je určeno, soubor je odesílán do dočasného umístění a výstup obsahuje adresu URL objektu blob.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-117">If **url** is specified, then the file is uploaded to a temporary location, and the output contains the URL of the blob.</span></span> <span data-ttu-id="3f9f6-118">Dočasné objekty BLOB se vyprázdní po 24 hodinách.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-118">Temporary blobs will be purged after 24 hours.</span></span> <span data-ttu-id="3f9f6-119">Výchozí hodnota je **soubor**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-119">The default value is **file**.</span></span>
- <span data-ttu-id="3f9f6-120">Hodnota `options.openMode` Určuje, jak je soubor pro čtení.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-120">The value of `options.openMode` determines how the file is read.</span></span> <span data-ttu-id="3f9f6-121">Pokud soubor je očekáván prostý text, zadejte **text**; v opačném, zadejte **binární**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-121">If the file is expected to be plain text, specify **text**; else, specify **binary**.</span></span> <span data-ttu-id="3f9f6-122">Výchozí hodnota je **text**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-122">The default value is **text**.</span></span>
- <span data-ttu-id="3f9f6-123">Pokud `options.uploadMode` je nastaven na **soubor** a `options.openMode` je nastaven na **binární**, výstup se kódování base64.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-123">If `options.uploadMode` is set to **file** and `options.openMode` is set to **binary**, the output is base64-encoded.</span></span>
- <span data-ttu-id="3f9f6-124">`options.encoding`Určuje kódování určené k použití při čtení souboru.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-124">`options.encoding` specifies the encoding to use when reading the file.</span></span> <span data-ttu-id="3f9f6-125">Výchozí hodnota je **UTF-8**a používá se pouze tehdy, když `options.openMode` je nastaven na **text**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-125">The default value is **UTF-8**, and is used only when `options.openMode` is set to **text**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="3f9f6-126">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="3f9f6-126">Sample output</span></span>
<span data-ttu-id="3f9f6-127">Pokud je hodnota false options.multiple a options.uploadMode je soubor, výstup obsahuje obsah souboru jako řetězec formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-127">If options.multiple is false and options.uploadMode is file, then the output contains the contents of the file as a JSON string:</span></span>

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

<span data-ttu-id="3f9f6-128">Pokud platí options.multiple and'options.uploadMode je soubor, pak výstup obsahuje obsah souborů jako pole JSON:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-128">If options.multiple is true and\`options.uploadMode is file, then the output contains the contents of the files as a JSON array:</span></span>

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

<span data-ttu-id="3f9f6-129">Pokud je hodnota false options.multiple a options.uploadMode je adresa url, výstup obsahuje adresu URL jako řetězec formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-129">If options.multiple is false and options.uploadMode is url, then the output contains a URL as a JSON string:</span></span>

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

<span data-ttu-id="3f9f6-130">Pokud je true options.multiple a options.uploadMode je adresa url, výstup obsahuje seznam adres URL jako pole JSON:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-130">If options.multiple is true and options.uploadMode is url, then the output contains a list of URLs as a JSON array:</span></span>
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

<span data-ttu-id="3f9f6-131">Při testování CreateUiDefinition, zkrátit některé prohlížeče (například Google Chrome) adresy URL vygenerovaných elementem Microsoft.Common.FileUpload v konzole prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-131">When testing a CreateUiDefinition, some browsers (like Google Chrome) truncate URLs generated by the Microsoft.Common.FileUpload element in the browser console.</span></span> <span data-ttu-id="3f9f6-132">Budete muset klikněte pravým tlačítkem na jednotlivé odkazy na kopírování úplné adresy URL.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-132">You may need to right-click individual links to copy the full URLs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3f9f6-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f9f6-133">Next steps</span></span>
* <span data-ttu-id="3f9f6-134">Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3f9f6-134">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="3f9f6-135">Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3f9f6-135">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="3f9f6-136">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="3f9f6-136">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
