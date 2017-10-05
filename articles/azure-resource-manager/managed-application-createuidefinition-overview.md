---
title: "Pochopení vytváření definice uživatelského rozhraní pro spravované aplikace Azure | Microsoft Docs"
description: "Popisuje postup vytvoření definice uživatelského rozhraní pro spravované aplikace Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 176b891538f85c5638a2321561c3d8bd377d245b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-createuidefinition"></a><span data-ttu-id="894a8-103">Začínáme s CreateUiDefinition</span><span class="sxs-lookup"><span data-stu-id="894a8-103">Getting started with CreateUiDefinition</span></span>
<span data-ttu-id="894a8-104">Tento dokument uvádí základní koncepty CreateUiDefinition, který se používá na portálu Azure ke generování uživatelského rozhraní pro vytváření spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="894a8-104">This document introduces the core concepts of a CreateUiDefinition, which is used by the Azure portal to generate the user interface for creating a managed application.</span></span>

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

<span data-ttu-id="894a8-105">CreateUiDefinition vždy obsahuje tři vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="894a8-105">A CreateUiDefinition always contains three properties:</span></span> 

* <span data-ttu-id="894a8-106">obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="894a8-106">handler</span></span>
* <span data-ttu-id="894a8-107">Verze</span><span class="sxs-lookup"><span data-stu-id="894a8-107">version</span></span>
* <span data-ttu-id="894a8-108">Parametry</span><span class="sxs-lookup"><span data-stu-id="894a8-108">parameters</span></span>

<span data-ttu-id="894a8-109">Pro spravované aplikace, musí být vždy obslužná rutina `Microsoft.Compute.MultiVm`, a nejnovější podporovaná verze je `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="894a8-109">For managed applications, handler should always be `Microsoft.Compute.MultiVm`, and the latest supported version is `0.1.2-preview`.</span></span>

<span data-ttu-id="894a8-110">Schéma vlastnosti parametry závisí na kombinaci zadaná obslužná rutina a verze.</span><span class="sxs-lookup"><span data-stu-id="894a8-110">The schema of the parameters property depends on the combination of the specified handler and version.</span></span> <span data-ttu-id="894a8-111">Pro spravované aplikace jsou podporované vlastnosti `basics`, `steps`, a `outputs`.</span><span class="sxs-lookup"><span data-stu-id="894a8-111">For managed applications, the supported properties are `basics`, `steps`, and `outputs`.</span></span> <span data-ttu-id="894a8-112">Vlastnosti základní informace a kroky obsahovat _elementy_ - jako textová pole a rozevírací seznamy – který se má zobrazit na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="894a8-112">The basics and steps properties contain the _elements_ - like textboxes and dropdowns - to be displayed in the Azure portal.</span></span> <span data-ttu-id="894a8-113">Vlastnost výstupy slouží k mapování výstup hodnot zadaných elementů na parametry šablony nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="894a8-113">The outputs property is used to map the output values of the specified elements to the parameters of the Azure Resource Manager deployment template.</span></span>

<span data-ttu-id="894a8-114">Včetně `$schema` je doporučená, ale volitelné.</span><span class="sxs-lookup"><span data-stu-id="894a8-114">Including `$schema` is recommended, but optional.</span></span> <span data-ttu-id="894a8-115">Je-li zadán, bude hodnota pro `version` musí shodovat s verzí v rámci `$schema` identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="894a8-115">If specified, the value for `version` must match the version within the `$schema` URI.</span></span>

## <a name="basics"></a><span data-ttu-id="894a8-116">Základy</span><span class="sxs-lookup"><span data-stu-id="894a8-116">Basics</span></span>
<span data-ttu-id="894a8-117">Základní informace o kroku je vždy první krok průvodce vygeneruje, když na portál Azure analyzuje CreateUiDefinition.</span><span class="sxs-lookup"><span data-stu-id="894a8-117">The Basics step is always the first step of the wizard generated when the Azure portal parses a CreateUiDefinition.</span></span> <span data-ttu-id="894a8-118">Kromě zobrazení prvky určené ve `basics`, portálu vloží prvky pro uživatelům si vybrat předplatné, skupinu prostředků a umístění pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="894a8-118">In addition to displaying the elements specified in `basics`, the portal injects elements for users to choose the subscription, resource group, and location for the deployment.</span></span> <span data-ttu-id="894a8-119">Obecně platí by měly prvky, které dotazu pro nasazení celou parametry, jako je třeba název clusteru nebo správce přihlašovacích údajů, přejděte v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="894a8-119">Generally, elements that query for deployment-wide parameters, like the name of a cluster or administrator credentials, should go in this step.</span></span>

<span data-ttu-id="894a8-120">Pokud elementu chování závisí na předplatné, skupinu prostředků nebo umístění uživatele, pak tento prvek nelze použít v základní informace.</span><span class="sxs-lookup"><span data-stu-id="894a8-120">If an element's behavior depends on the user's subscription, resource group, or location, then that element can't be used in basics.</span></span> <span data-ttu-id="894a8-121">Například **Microsoft.Compute.SizeSelector** závisí na předplatném a umístění určit seznam dostupných velikostí uživatele.</span><span class="sxs-lookup"><span data-stu-id="894a8-121">For example, **Microsoft.Compute.SizeSelector** depends on the user's subscription and location to determine the list of available sizes.</span></span> <span data-ttu-id="894a8-122">Proto **Microsoft.Compute.SizeSelector** lze použít pouze v krocích.</span><span class="sxs-lookup"><span data-stu-id="894a8-122">Therefore, **Microsoft.Compute.SizeSelector** can only be used in steps.</span></span> <span data-ttu-id="894a8-123">Obecně platí, pouze elementy v **Microsoft.Common** oboru názvů lze použít v základní informace.</span><span class="sxs-lookup"><span data-stu-id="894a8-123">Generally, only elements in the **Microsoft.Common** namespace can be used in basics.</span></span> <span data-ttu-id="894a8-124">I když některé prvky v jiných oborech názvů (jako je **Microsoft.Compute.Credentials**), nemáte jsou závislé na kontextu uživatele, jsou stále povoleny.</span><span class="sxs-lookup"><span data-stu-id="894a8-124">Although some elements in other namespaces (like **Microsoft.Compute.Credentials**) that don't depend on the user's context, are still allowed.</span></span>

## <a name="steps"></a><span data-ttu-id="894a8-125">Kroky</span><span class="sxs-lookup"><span data-stu-id="894a8-125">Steps</span></span>
<span data-ttu-id="894a8-126">Vlastnost kroky může obsahovat nula nebo více další kroky k zobrazení po základy, z nichž každý obsahuje jeden či více elementů.</span><span class="sxs-lookup"><span data-stu-id="894a8-126">The steps property can contain zero or more additional steps to display after basics, each of which contains one or more elements.</span></span> <span data-ttu-id="894a8-127">Zvažte přidání kroků / role nebo vrstvě nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="894a8-127">Consider adding steps per role or tier of the application being deployed.</span></span> <span data-ttu-id="894a8-128">Například přidáte krok pro vstupy pro hlavní uzly a krok pro uzly pracovního procesu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="894a8-128">For example, add a step for inputs for the master nodes, and a step for the worker nodes in a cluster.</span></span>

## <a name="outputs"></a><span data-ttu-id="894a8-129">Výstupy</span><span class="sxs-lookup"><span data-stu-id="894a8-129">Outputs</span></span>
<span data-ttu-id="894a8-130">Používá portál Azure `outputs` vlastnost pro mapování elementů od `basics` a `steps` na parametry šablony nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="894a8-130">The Azure portal uses the `outputs` property to map elements from `basics` and `steps` to the parameters of the Azure Resource Manager deployment template.</span></span> <span data-ttu-id="894a8-131">Názvy parametrů šablony jsou klíče tohoto slovníku a hodnoty jsou vlastnosti objektů výstup z odkazované elementy.</span><span class="sxs-lookup"><span data-stu-id="894a8-131">The keys of this dictionary are the names of the template parameters, and the values are properties of the output objects from the referenced elements.</span></span>

## <a name="functions"></a><span data-ttu-id="894a8-132">Funkce</span><span class="sxs-lookup"><span data-stu-id="894a8-132">Functions</span></span>
<span data-ttu-id="894a8-133">Podobně jako [funkce šablon](resource-group-template-functions.md) v Azure Resource Manager (v syntaxi a funkce i), CreateUiDefinition poskytuje funkce pro práci s vstupy a výstupy elementy, stejně jako funkce, jako je například podmíněné příkazy.</span><span class="sxs-lookup"><span data-stu-id="894a8-133">Similar to [template functions](resource-group-template-functions.md) in Azure Resource Manager (both in syntax and functionality), CreateUiDefinition provides functions for working with elements' inputs and outputs, as well as features such as conditionals.</span></span>

## <a name="next-steps"></a><span data-ttu-id="894a8-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="894a8-134">Next steps</span></span>
<span data-ttu-id="894a8-135">CreateUiDefinition samotné má jednoduché schéma.</span><span class="sxs-lookup"><span data-stu-id="894a8-135">CreateUiDefinition itself has a simple schema.</span></span> <span data-ttu-id="894a8-136">Skutečné hloubka ho pochází z všechny podporované elementy a funkce, které udivujících podrobně popisují v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="894a8-136">The real depth of it comes from all the supported elements and functions, which the following documents describe in wondrous detail:</span></span>

- [<span data-ttu-id="894a8-137">Elementy</span><span class="sxs-lookup"><span data-stu-id="894a8-137">Elements</span></span>](managed-application-createuidefinition-elements.md)
- [<span data-ttu-id="894a8-138">Functions</span><span class="sxs-lookup"><span data-stu-id="894a8-138">Functions</span></span>](managed-application-createuidefinition-functions.md)

<span data-ttu-id="894a8-139">Zde jsou k dispozici aktuální schéma JSON pro CreateUiDefinition: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span><span class="sxs-lookup"><span data-stu-id="894a8-139">A current JSON schema for CreateUiDefinition is available here: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span></span> 

<span data-ttu-id="894a8-140">Novější verze bude k dispozici ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="894a8-140">Later versions will be available at the same location.</span></span> <span data-ttu-id="894a8-141">Nahraďte `0.1.2-preview` část adresy URL a `version` hodnotu s identifikátor verze, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="894a8-141">Replace the `0.1.2-preview` portion of the URL and the `version` value with the version identifier you intend to use.</span></span> <span data-ttu-id="894a8-142">Identifikátory aktuálně podporované verze jsou `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, a `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="894a8-142">The currently supported version identifiers are `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, and `0.1.2-preview`.</span></span>