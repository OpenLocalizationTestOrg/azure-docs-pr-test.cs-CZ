---
title: "aaaUnderstand vytváření definice uživatelského rozhraní pro spravované aplikace Azure | Microsoft Docs"
description: "Popisuje, jak toocreate definice uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: d53ddf438c24d5a6cb8dd53ca0b4694ab0462515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-createuidefinition"></a><span data-ttu-id="77674-103">Začínáme s CreateUiDefinition</span><span class="sxs-lookup"><span data-stu-id="77674-103">Getting started with CreateUiDefinition</span></span>
<span data-ttu-id="77674-104">Tento dokument uvádí základní koncepty hello CreateUiDefinition, který je používán hello Azure portálu toogenerate hello uživatelské rozhraní pro vytváření spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="77674-104">This document introduces hello core concepts of a CreateUiDefinition, which is used by hello Azure portal toogenerate hello user interface for creating a managed application.</span></span>

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

<span data-ttu-id="77674-105">CreateUiDefinition vždy obsahuje tři vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="77674-105">A CreateUiDefinition always contains three properties:</span></span> 

* <span data-ttu-id="77674-106">obslužné rutiny</span><span class="sxs-lookup"><span data-stu-id="77674-106">handler</span></span>
* <span data-ttu-id="77674-107">Verze</span><span class="sxs-lookup"><span data-stu-id="77674-107">version</span></span>
* <span data-ttu-id="77674-108">parameters</span><span class="sxs-lookup"><span data-stu-id="77674-108">parameters</span></span>

<span data-ttu-id="77674-109">Pro spravované aplikace, musí být vždy obslužná rutina `Microsoft.Compute.MultiVm`, je hello nejnovější podporované verze a `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="77674-109">For managed applications, handler should always be `Microsoft.Compute.MultiVm`, and hello latest supported version is `0.1.2-preview`.</span></span>

<span data-ttu-id="77674-110">schéma Hello hello parametry vlastnosti závisí na kombinaci hello zadaná obslužná rutina hello a verze.</span><span class="sxs-lookup"><span data-stu-id="77674-110">hello schema of hello parameters property depends on hello combination of hello specified handler and version.</span></span> <span data-ttu-id="77674-111">Pro spravované aplikace jsou vlastnosti hello podporované `basics`, `steps`, a `outputs`.</span><span class="sxs-lookup"><span data-stu-id="77674-111">For managed applications, hello supported properties are `basics`, `steps`, and `outputs`.</span></span> <span data-ttu-id="77674-112">Hello základní informace a kroky obsahují hello _elementy_ – stejně jako textová pole a rozevírací seznamy - toobe zobrazeny v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="77674-112">hello basics and steps properties contain hello _elements_ - like textboxes and dropdowns - toobe displayed in hello Azure portal.</span></span> <span data-ttu-id="77674-113">výstupy Hello, vlastnost je hodnoty výstup hello použité toomap hello zadaných elementů toohello parametry šablony nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="77674-113">hello outputs property is used toomap hello output values of hello specified elements toohello parameters of hello Azure Resource Manager deployment template.</span></span>

<span data-ttu-id="77674-114">Včetně `$schema` je doporučená, ale volitelné.</span><span class="sxs-lookup"><span data-stu-id="77674-114">Including `$schema` is recommended, but optional.</span></span> <span data-ttu-id="77674-115">-Li zadána, hello hodnotu `version` musí odpovídat verzi hello v rámci hello `$schema` identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="77674-115">If specified, hello value for `version` must match hello version within hello `$schema` URI.</span></span>

## <a name="basics"></a><span data-ttu-id="77674-116">Základy</span><span class="sxs-lookup"><span data-stu-id="77674-116">Basics</span></span>
<span data-ttu-id="77674-117">Krok základy Hello je vždy první krok hello hello průvodce vygeneruje, když se analyzuje CreateUiDefinition hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="77674-117">hello Basics step is always hello first step of hello wizard generated when hello Azure portal parses a CreateUiDefinition.</span></span> <span data-ttu-id="77674-118">Kromě toho toodisplaying hello elementy zadaný v `basics`, portál hello vloží prvky pro uživatele toochoose hello předplatné, skupinu prostředků a umístění pro nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="77674-118">In addition toodisplaying hello elements specified in `basics`, hello portal injects elements for users toochoose hello subscription, resource group, and location for hello deployment.</span></span> <span data-ttu-id="77674-119">Obecně platí by měly prvky, které se dotázat na úrovni nasazení parametry, jako hello název clusteru nebo správce přihlašovacích údajů, přejděte v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="77674-119">Generally, elements that query for deployment-wide parameters, like hello name of a cluster or administrator credentials, should go in this step.</span></span>

<span data-ttu-id="77674-120">Pokud elementu chování závisí na předplatné hello uživatele, skupinu prostředků nebo umístění, nelze v základy použít daný element.</span><span class="sxs-lookup"><span data-stu-id="77674-120">If an element's behavior depends on hello user's subscription, resource group, or location, then that element can't be used in basics.</span></span> <span data-ttu-id="77674-121">Například **Microsoft.Compute.SizeSelector** závisí na hello uživatele předplatném a umístění toodetermine hello seznam dostupných velikostí.</span><span class="sxs-lookup"><span data-stu-id="77674-121">For example, **Microsoft.Compute.SizeSelector** depends on hello user's subscription and location toodetermine hello list of available sizes.</span></span> <span data-ttu-id="77674-122">Proto **Microsoft.Compute.SizeSelector** lze použít pouze v krocích.</span><span class="sxs-lookup"><span data-stu-id="77674-122">Therefore, **Microsoft.Compute.SizeSelector** can only be used in steps.</span></span> <span data-ttu-id="77674-123">Obecně platí, pouze elementy v hello **Microsoft.Common** oboru názvů lze použít v základní informace.</span><span class="sxs-lookup"><span data-stu-id="77674-123">Generally, only elements in hello **Microsoft.Common** namespace can be used in basics.</span></span> <span data-ttu-id="77674-124">I když některé prvky v jiných oborech názvů (jako je **Microsoft.Compute.Credentials**), nemáte jsou závislé na kontextu hello uživatele, jsou stále povoleny.</span><span class="sxs-lookup"><span data-stu-id="77674-124">Although some elements in other namespaces (like **Microsoft.Compute.Credentials**) that don't depend on hello user's context, are still allowed.</span></span>

## <a name="steps"></a><span data-ttu-id="77674-125">Kroky</span><span class="sxs-lookup"><span data-stu-id="77674-125">Steps</span></span>
<span data-ttu-id="77674-126">Vlastnost kroky Hello může obsahovat nula nebo více toodisplay další kroky po základy, z nichž každý obsahuje jeden či více elementů.</span><span class="sxs-lookup"><span data-stu-id="77674-126">hello steps property can contain zero or more additional steps toodisplay after basics, each of which contains one or more elements.</span></span> <span data-ttu-id="77674-127">Zvažte přidání kroků / role nebo vrstvě aplikace hello nasazuje.</span><span class="sxs-lookup"><span data-stu-id="77674-127">Consider adding steps per role or tier of hello application being deployed.</span></span> <span data-ttu-id="77674-128">Například přidáte krok pro vstupy pro hello hlavní uzly a krok pro hello uzly pracovního procesu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="77674-128">For example, add a step for inputs for hello master nodes, and a step for hello worker nodes in a cluster.</span></span>

## <a name="outputs"></a><span data-ttu-id="77674-129">Výstupy</span><span class="sxs-lookup"><span data-stu-id="77674-129">Outputs</span></span>
<span data-ttu-id="77674-130">Hello portál Azure používá hello `outputs` vlastnost toomap elementy z `basics` a `steps` toohello parametry šablony nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="77674-130">hello Azure portal uses hello `outputs` property toomap elements from `basics` and `steps` toohello parameters of hello Azure Resource Manager deployment template.</span></span> <span data-ttu-id="77674-131">Hello klíče tohoto slovníku jsou hello názvy parametrů šablony hello a hello hodnoty jsou vlastnosti objektů hello výstup z elementů hello odkazuje.</span><span class="sxs-lookup"><span data-stu-id="77674-131">hello keys of this dictionary are hello names of hello template parameters, and hello values are properties of hello output objects from hello referenced elements.</span></span>

## <a name="functions"></a><span data-ttu-id="77674-132">Funkce</span><span class="sxs-lookup"><span data-stu-id="77674-132">Functions</span></span>
<span data-ttu-id="77674-133">Podobně jako příliš[funkce šablon](resource-group-template-functions.md) v Azure Resource Manager (v syntaxi a funkce i), CreateUiDefinition poskytuje funkce pro práci s vstupy a výstupy elementy, stejně jako funkce, jako je například podmíněné příkazy.</span><span class="sxs-lookup"><span data-stu-id="77674-133">Similar too[template functions](resource-group-template-functions.md) in Azure Resource Manager (both in syntax and functionality), CreateUiDefinition provides functions for working with elements' inputs and outputs, as well as features such as conditionals.</span></span>

## <a name="next-steps"></a><span data-ttu-id="77674-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77674-134">Next steps</span></span>
<span data-ttu-id="77674-135">CreateUiDefinition samotné má jednoduché schéma.</span><span class="sxs-lookup"><span data-stu-id="77674-135">CreateUiDefinition itself has a simple schema.</span></span> <span data-ttu-id="77674-136">skutečné hloubka Hello je pochází z všechny elementy hello podporována a funkce, které hello následující dokumenty popisují udivujících podrobně:</span><span class="sxs-lookup"><span data-stu-id="77674-136">hello real depth of it comes from all hello supported elements and functions, which hello following documents describe in wondrous detail:</span></span>

- [<span data-ttu-id="77674-137">Elementy</span><span class="sxs-lookup"><span data-stu-id="77674-137">Elements</span></span>](managed-application-createuidefinition-elements.md)
- [<span data-ttu-id="77674-138">Functions</span><span class="sxs-lookup"><span data-stu-id="77674-138">Functions</span></span>](managed-application-createuidefinition-functions.md)

<span data-ttu-id="77674-139">Zde jsou k dispozici aktuální schéma JSON pro CreateUiDefinition: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span><span class="sxs-lookup"><span data-stu-id="77674-139">A current JSON schema for CreateUiDefinition is available here: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.</span></span> 

<span data-ttu-id="77674-140">Novější verze bude k dispozici v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="77674-140">Later versions will be available at hello same location.</span></span> <span data-ttu-id="77674-141">Nahraďte hello `0.1.2-preview` část adresy URL hello a hello `version` hodnotu s identifikátorem verze hello hodláte toouse.</span><span class="sxs-lookup"><span data-stu-id="77674-141">Replace hello `0.1.2-preview` portion of hello URL and hello `version` value with hello version identifier you intend toouse.</span></span> <span data-ttu-id="77674-142">identifikátory Hello aktuálně podporované verze jsou `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, a `0.1.2-preview`.</span><span class="sxs-lookup"><span data-stu-id="77674-142">hello currently supported version identifiers are `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, and `0.1.2-preview`.</span></span>
