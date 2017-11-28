---
title: "element uživatelského rozhraní StorageAccountSelector spravované aplikace aaaAzure | Microsoft Docs"
description: "Popisuje hello elementu Microsoft.Storage.StorageAccountSelector uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: a2c9545feed4c4afb3c64b30b42c94d5382a108d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a><span data-ttu-id="32f2f-103">Element Microsoft.Storage.StorageAccountSelector uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="32f2f-103">Microsoft.Storage.StorageAccountSelector UI element</span></span>
<span data-ttu-id="32f2f-104">Ovládací prvek pro výběr účtu nový nebo existující úložiště.</span><span class="sxs-lookup"><span data-stu-id="32f2f-104">A control for selecting a new or existing storage account.</span></span> <span data-ttu-id="32f2f-105">Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="32f2f-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="32f2f-106">Ukázka uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="32f2f-106">UI sample</span></span>
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a><span data-ttu-id="32f2f-108">Schéma</span><span class="sxs-lookup"><span data-stu-id="32f2f-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="32f2f-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="32f2f-109">Remarks</span></span>
- <span data-ttu-id="32f2f-110">-Li zadána, `defaultValue.name` dojde k automatickému ověření jedinečnosti.</span><span class="sxs-lookup"><span data-stu-id="32f2f-110">If specified, `defaultValue.name` is automatically validated for uniqueness.</span></span> <span data-ttu-id="32f2f-111">Pokud název účtu úložiště hello není jedinečný, musí uživatel hello zadejte jiný název nebo vybrat existující účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="32f2f-111">If hello storage account name is not unique, hello user must specify a different name or choose an existing storage account.</span></span>
- <span data-ttu-id="32f2f-112">Výchozí hodnota pro Hello `defaultValue.type` je **Premium_LRS**.</span><span class="sxs-lookup"><span data-stu-id="32f2f-112">hello default value for `defaultValue.type` is **Premium_LRS**.</span></span>
- <span data-ttu-id="32f2f-113">Žádný typ, nebyly zadány v `constraints.allowedTypes` skryt a jakýmikoli nebyly zadány v `constraints.excludedTypes` se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="32f2f-113">Any type not specified in `constraints.allowedTypes` is hidden, and any type not specified in `constraints.excludedTypes` is shown.</span></span>
<span data-ttu-id="32f2f-114">`constraints.allowedTypes`a `constraints.excludedTypes` obě jsou nepovinné, ale nelze používat současně.</span><span class="sxs-lookup"><span data-stu-id="32f2f-114">`constraints.allowedTypes` and `constraints.excludedTypes` are both optional, but cannot be used simultaneously.</span></span>
- <span data-ttu-id="32f2f-115">Pokud `options.hideExisting` je **true**, uživatel hello nelze vybrat existující účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="32f2f-115">If `options.hideExisting` is **true**, hello user can't choose an existing storage account.</span></span> <span data-ttu-id="32f2f-116">Hello výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="32f2f-116">hello default value is **false**.</span></span>


## <a name="sample-output"></a><span data-ttu-id="32f2f-117">Ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="32f2f-117">Sample output</span></span>
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a><span data-ttu-id="32f2f-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32f2f-118">Next steps</span></span>
* <span data-ttu-id="32f2f-119">Úvod toomanaged aplikace naleznete v [Azure spravovaných aplikací – přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32f2f-119">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="32f2f-120">Úvod toocreating uživatelského rozhraní definice naleznete v tématu [Začínáme s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32f2f-120">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="32f2f-121">Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="32f2f-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
