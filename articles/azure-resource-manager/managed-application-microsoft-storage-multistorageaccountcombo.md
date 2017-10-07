---
title: "element uživatelského rozhraní MultiStorageAccountCombo spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Storage.MultiStorageAccountCombo uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: 765be145b61c3dbf0a035a7a00aa18eee464a3eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a><span data-ttu-id="86fc6-103">Element Microsoft.Storage.MultiStorageAccountCombo uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="86fc6-103">Microsoft.Storage.MultiStorageAccountCombo UI element</span></span>
<span data-ttu-id="86fc6-104">Skupina ovládacích prvků pro vytváření více účtů úložiště, jejichž názvy začínají s předponou běžné.</span><span class="sxs-lookup"><span data-stu-id="86fc6-104">A group of controls for creating multiple storage accounts, with names that start with a common prefix.</span></span> <span data-ttu-id="86fc6-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="86fc6-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="86fc6-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="86fc6-106">UI sample</span></span>
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a><span data-ttu-id="86fc6-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="86fc6-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="86fc6-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="86fc6-109">Remarks</span></span>
- <span data-ttu-id="86fc6-110">Hello hodnota `defaultValue.prefix` je zřetězen s jeden nebo více celých čísel toogenerate hello posloupnost názvy účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="86fc6-110">hello value for `defaultValue.prefix` is concatenated with one or more integers toogenerate hello sequence of storage account names.</span></span> <span data-ttu-id="86fc6-111">Například pokud `defaultValue.prefix` je **foobar** a `count` je **2**, pak názvy účtů úložiště **foobar1** a **foobar2** se generují.</span><span class="sxs-lookup"><span data-stu-id="86fc6-111">For example, if `defaultValue.prefix` is **foobar** and `count` is **2**, then storage account names **foobar1** and **foobar2** are generated.</span></span> <span data-ttu-id="86fc6-112">Názvy účtů úložiště generovaného ověření jedinečnosti automaticky.</span><span class="sxs-lookup"><span data-stu-id="86fc6-112">Generated storage account names are validated for uniqueness automatically.</span></span>
- <span data-ttu-id="86fc6-113">názvy účtů úložiště Hello jsou generovány lexicographically podle `count`.</span><span class="sxs-lookup"><span data-stu-id="86fc6-113">hello storage account names are generated lexicographically based on `count`.</span></span> <span data-ttu-id="86fc6-114">Například pokud `count` je 10 a názvy účtů úložiště hello končit celá čísla 2 číslice (01, 02, 03, atd.).</span><span class="sxs-lookup"><span data-stu-id="86fc6-114">For example, if `count` is 10, then hello storage account names end with 2-digit integers (01, 02, 03, etc.).</span></span>
- <span data-ttu-id="86fc6-115">Výchozí hodnota pro Hello `defaultValue.prefix` je **null**a pro `defaultValue.type` je **Premium_LRS**.</span><span class="sxs-lookup"><span data-stu-id="86fc6-115">hello default value for `defaultValue.prefix` is **null**, and for `defaultValue.type` is **Premium_LRS**.</span></span>
- <span data-ttu-id="86fc6-116">Žádný typ, nebyly zadány v `constraints.allowedTypes` skryt a jakýmikoli nebyly zadány v `constraints.excludedTypes` se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="86fc6-116">Any type not specified in `constraints.allowedTypes` is hidden, and any type not specified in `constraints.excludedTypes` is shown.</span></span>
<span data-ttu-id="86fc6-117">`constraints.allowedTypes`a `constraints.excludedTypes` obě jsou nepovinné, ale nelze používat současně.</span><span class="sxs-lookup"><span data-stu-id="86fc6-117">`constraints.allowedTypes` and `constraints.excludedTypes` are both optional, but cannot be used simultaneously.</span></span>
- <span data-ttu-id="86fc6-118">V přidání toogenerating názvy účtů úložiště `count` je použité tooset odpovídající multiplikátor pro hello element.</span><span class="sxs-lookup"><span data-stu-id="86fc6-118">In addition toogenerating storage account names, `count` is used tooset the appropriate multiplier for hello element.</span></span> <span data-ttu-id="86fc6-119">Podporuje statické hodnoty, jako je třeba **2**, nebo jako dynamické hodnoty z jiný element `[steps('step1').storageAccountCount]`.</span><span class="sxs-lookup"><span data-stu-id="86fc6-119">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').storageAccountCount]`.</span></span> <span data-ttu-id="86fc6-120">Hello výchozí hodnota je **1**.</span><span class="sxs-lookup"><span data-stu-id="86fc6-120">hello default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="86fc6-121">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="86fc6-121">Sample output</span></span>
```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a><span data-ttu-id="86fc6-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86fc6-122">Next steps</span></span>
* <span data-ttu-id="86fc6-123">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="86fc6-123">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="86fc6-124">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="86fc6-124">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="86fc6-125">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="86fc6-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
