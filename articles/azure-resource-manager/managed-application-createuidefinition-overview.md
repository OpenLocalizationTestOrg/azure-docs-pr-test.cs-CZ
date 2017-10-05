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
# <a name="getting-started-with-createuidefinition"></a>Začínáme s CreateUiDefinition
Tento dokument uvádí základní koncepty CreateUiDefinition, který se používá na portálu Azure ke generování uživatelského rozhraní pro vytváření spravované aplikace.

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

CreateUiDefinition vždy obsahuje tři vlastnosti: 

* obslužné rutiny
* Verze
* Parametry

Pro spravované aplikace, musí být vždy obslužná rutina `Microsoft.Compute.MultiVm`, a nejnovější podporovaná verze je `0.1.2-preview`.

Schéma vlastnosti parametry závisí na kombinaci zadaná obslužná rutina a verze. Pro spravované aplikace jsou podporované vlastnosti `basics`, `steps`, a `outputs`. Vlastnosti základní informace a kroky obsahovat _elementy_ - jako textová pole a rozevírací seznamy – který se má zobrazit na portálu Azure. Vlastnost výstupy slouží k mapování výstup hodnot zadaných elementů na parametry šablony nasazení Azure Resource Manager.

Včetně `$schema` je doporučená, ale volitelné. Je-li zadán, bude hodnota pro `version` musí shodovat s verzí v rámci `$schema` identifikátor URI.

## <a name="basics"></a>Základy
Základní informace o kroku je vždy první krok průvodce vygeneruje, když na portál Azure analyzuje CreateUiDefinition. Kromě zobrazení prvky určené ve `basics`, portálu vloží prvky pro uživatelům si vybrat předplatné, skupinu prostředků a umístění pro nasazení. Obecně platí by měly prvky, které dotazu pro nasazení celou parametry, jako je třeba název clusteru nebo správce přihlašovacích údajů, přejděte v tomto kroku.

Pokud elementu chování závisí na předplatné, skupinu prostředků nebo umístění uživatele, pak tento prvek nelze použít v základní informace. Například **Microsoft.Compute.SizeSelector** závisí na předplatném a umístění určit seznam dostupných velikostí uživatele. Proto **Microsoft.Compute.SizeSelector** lze použít pouze v krocích. Obecně platí, pouze elementy v **Microsoft.Common** oboru názvů lze použít v základní informace. I když některé prvky v jiných oborech názvů (jako je **Microsoft.Compute.Credentials**), nemáte jsou závislé na kontextu uživatele, jsou stále povoleny.

## <a name="steps"></a>Kroky
Vlastnost kroky může obsahovat nula nebo více další kroky k zobrazení po základy, z nichž každý obsahuje jeden či více elementů. Zvažte přidání kroků / role nebo vrstvě nasazení aplikace. Například přidáte krok pro vstupy pro hlavní uzly a krok pro uzly pracovního procesu v clusteru.

## <a name="outputs"></a>Výstupy
Používá portál Azure `outputs` vlastnost pro mapování elementů od `basics` a `steps` na parametry šablony nasazení Azure Resource Manager. Názvy parametrů šablony jsou klíče tohoto slovníku a hodnoty jsou vlastnosti objektů výstup z odkazované elementy.

## <a name="functions"></a>Funkce
Podobně jako [funkce šablon](resource-group-template-functions.md) v Azure Resource Manager (v syntaxi a funkce i), CreateUiDefinition poskytuje funkce pro práci s vstupy a výstupy elementy, stejně jako funkce, jako je například podmíněné příkazy.

## <a name="next-steps"></a>Další kroky
CreateUiDefinition samotné má jednoduché schéma. Skutečné hloubka ho pochází z všechny podporované elementy a funkce, které udivujících podrobně popisují v následujících dokumentech:

- [Elementy](managed-application-createuidefinition-elements.md)
- [Functions](managed-application-createuidefinition-functions.md)

Zde jsou k dispozici aktuální schéma JSON pro CreateUiDefinition: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json. 

Novější verze bude k dispozici ve stejném umístění. Nahraďte `0.1.2-preview` část adresy URL a `version` hodnotu s identifikátor verze, který chcete použít. Identifikátory aktuálně podporované verze jsou `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, a `0.1.2-preview`.