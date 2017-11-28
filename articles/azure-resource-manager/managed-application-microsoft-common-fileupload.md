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
# <a name="microsoftcommonfileupload-ui-element"></a><span data-ttu-id="71e08-103">Element Microsoft.Common.FileUpload uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="71e08-103">Microsoft.Common.FileUpload UI element</span></span>
<span data-ttu-id="71e08-104">Ovládací prvek, který umožňuje toospecify uživatele jeden nebo více souborů tooupload.</span><span class="sxs-lookup"><span data-stu-id="71e08-104">A control that allows a user toospecify one or more files tooupload.</span></span> <span data-ttu-id="71e08-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="71e08-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="71e08-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="71e08-106">UI sample</span></span>
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a><span data-ttu-id="71e08-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="71e08-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="71e08-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="71e08-109">Remarks</span></span>
- <span data-ttu-id="71e08-110">`constraints.accept`Určuje hello typy souborů, které se zobrazují v dialogovém okně prohlížeče hello souboru.</span><span class="sxs-lookup"><span data-stu-id="71e08-110">`constraints.accept` specifies hello types of files that are shown in hello browser's file dialog.</span></span> <span data-ttu-id="71e08-111">V tématu hello [specifikace HTML5](http://www.w3.org/TR/html5/forms.html#attr-input-accept) pro povolené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="71e08-111">See hello [HTML5 specification](http://www.w3.org/TR/html5/forms.html#attr-input-accept) for allowed values.</span></span> <span data-ttu-id="71e08-112">Hello výchozí hodnota je **null**.</span><span class="sxs-lookup"><span data-stu-id="71e08-112">hello default value is **null**.</span></span>
- <span data-ttu-id="71e08-113">Pokud `options.multiple` je nastaven příliš**true**, hello uživatel tooselect více než jeden soubor v dialogovém okně prohlížeče hello souboru.</span><span class="sxs-lookup"><span data-stu-id="71e08-113">If `options.multiple` is set too**true**, hello user is allowed tooselect more than one file in hello browser's file dialog.</span></span> <span data-ttu-id="71e08-114">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="71e08-114">hello default value is **false**.</span></span>
- <span data-ttu-id="71e08-115">Tento element podporuje odesílání souborů ve dvou režimech na základě hodnoty hello `options.uploadMode`.</span><span class="sxs-lookup"><span data-stu-id="71e08-115">This element supports uploading files in two modes based on hello value of `options.uploadMode`.</span></span> <span data-ttu-id="71e08-116">Pokud **soubor** není zadaný, výstup hello obsahuje hello obsah souboru hello jako objekt blob.</span><span class="sxs-lookup"><span data-stu-id="71e08-116">If **file** is specified, hello output contains hello contents of hello file as a blob.</span></span> <span data-ttu-id="71e08-117">Pokud **url** je určeno, soubor hello nahrané tooa dočasného umístění a výstup hello obsahuje hello URL objektu hello blob.</span><span class="sxs-lookup"><span data-stu-id="71e08-117">If **url** is specified, then hello file is uploaded tooa temporary location, and hello output contains hello URL of hello blob.</span></span> <span data-ttu-id="71e08-118">Dočasné objekty BLOB se vyprázdní po 24 hodinách.</span><span class="sxs-lookup"><span data-stu-id="71e08-118">Temporary blobs will be purged after 24 hours.</span></span> <span data-ttu-id="71e08-119">Hello výchozí hodnota je **soubor**.</span><span class="sxs-lookup"><span data-stu-id="71e08-119">hello default value is **file**.</span></span>
- <span data-ttu-id="71e08-120">Hello hodnota `options.openMode` Určuje, jak je hello souboru pro čtení.</span><span class="sxs-lookup"><span data-stu-id="71e08-120">hello value of `options.openMode` determines how hello file is read.</span></span> <span data-ttu-id="71e08-121">Pokud soubor hello očekávané toobe prostý text, zadejte **text**; v opačném, zadejte **binární**.</span><span class="sxs-lookup"><span data-stu-id="71e08-121">If hello file is expected toobe plain text, specify **text**; else, specify **binary**.</span></span> <span data-ttu-id="71e08-122">Hello výchozí hodnota je **text**.</span><span class="sxs-lookup"><span data-stu-id="71e08-122">hello default value is **text**.</span></span>
- <span data-ttu-id="71e08-123">Pokud `options.uploadMode` je nastaven příliš**soubor** a `options.openMode` je nastaven příliš**binární**, výstup hello se kódování base64.</span><span class="sxs-lookup"><span data-stu-id="71e08-123">If `options.uploadMode` is set too**file** and `options.openMode` is set too**binary**, hello output is base64-encoded.</span></span>
- <span data-ttu-id="71e08-124">`options.encoding`Určuje kódování toouse hello při čtení souboru hello.</span><span class="sxs-lookup"><span data-stu-id="71e08-124">`options.encoding` specifies hello encoding toouse when reading hello file.</span></span> <span data-ttu-id="71e08-125">Hello výchozí hodnota je **UTF-8**a používá se pouze tehdy, když `options.openMode` je nastaven příliš**text**.</span><span class="sxs-lookup"><span data-stu-id="71e08-125">hello default value is **UTF-8**, and is used only when `options.openMode` is set too**text**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="71e08-126">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="71e08-126">Sample output</span></span>
<span data-ttu-id="71e08-127">Pokud je hodnota false options.multiple a options.uploadMode je soubor, výstup obsahuje hello obsah souboru hello jako řetězec formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="71e08-127">If options.multiple is false and options.uploadMode is file, then the output contains hello contents of hello file as a JSON string:</span></span>

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

<span data-ttu-id="71e08-128">Pokud platí options.multiple and'options.uploadMode je soubor, pak výstup obsahuje hello obsah souborů hello jako pole JSON:</span><span class="sxs-lookup"><span data-stu-id="71e08-128">If options.multiple is true and\`options.uploadMode is file, then the output contains hello contents of hello files as a JSON array:</span></span>

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

<span data-ttu-id="71e08-129">Pokud je hodnota false options.multiple a options.uploadMode je adresa url, výstup obsahuje adresu URL jako řetězec formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="71e08-129">If options.multiple is false and options.uploadMode is url, then the output contains a URL as a JSON string:</span></span>

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

<span data-ttu-id="71e08-130">Pokud je true options.multiple a options.uploadMode je adresa url, výstup obsahuje seznam adres URL jako pole JSON:</span><span class="sxs-lookup"><span data-stu-id="71e08-130">If options.multiple is true and options.uploadMode is url, then the output contains a list of URLs as a JSON array:</span></span>
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

<span data-ttu-id="71e08-131">Při testování CreateUiDefinition, zkrátit některé prohlížeče (například Google Chrome) adresy URL vygenerovaných elementem hello Microsoft.Common.FileUpload v konzole prohlížeče hello.</span><span class="sxs-lookup"><span data-stu-id="71e08-131">When testing a CreateUiDefinition, some browsers (like Google Chrome) truncate URLs generated by hello Microsoft.Common.FileUpload element in hello browser console.</span></span> <span data-ttu-id="71e08-132">Může být nutné tooright kliknutím jednotlivé odkazy toocopy hello úplné adresy URL.</span><span class="sxs-lookup"><span data-stu-id="71e08-132">You may need tooright-click individual links toocopy hello full URLs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="71e08-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71e08-133">Next steps</span></span>
* <span data-ttu-id="71e08-134">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71e08-134">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="71e08-135">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71e08-135">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="71e08-136">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="71e08-136">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
