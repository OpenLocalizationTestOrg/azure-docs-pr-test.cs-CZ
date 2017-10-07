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
# <a name="getting-started-with-createuidefinition"></a>Začínáme s CreateUiDefinition
Tento dokument uvádí základní koncepty hello CreateUiDefinition, který je používán hello Azure portálu toogenerate hello uživatelské rozhraní pro vytváření spravované aplikace.

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
* parameters

Pro spravované aplikace, musí být vždy obslužná rutina `Microsoft.Compute.MultiVm`, je hello nejnovější podporované verze a `0.1.2-preview`.

schéma Hello hello parametry vlastnosti závisí na kombinaci hello zadaná obslužná rutina hello a verze. Pro spravované aplikace jsou vlastnosti hello podporované `basics`, `steps`, a `outputs`. Hello základní informace a kroky obsahují hello _elementy_ – stejně jako textová pole a rozevírací seznamy - toobe zobrazeny v hello portálu Azure. výstupy Hello, vlastnost je hodnoty výstup hello použité toomap hello zadaných elementů toohello parametry šablony nasazení Azure Resource Manager hello.

Včetně `$schema` je doporučená, ale volitelné. -Li zadána, hello hodnotu `version` musí odpovídat verzi hello v rámci hello `$schema` identifikátor URI.

## <a name="basics"></a>Základy
Krok základy Hello je vždy první krok hello hello průvodce vygeneruje, když se analyzuje CreateUiDefinition hello portálu Azure. Kromě toho toodisplaying hello elementy zadaný v `basics`, portál hello vloží prvky pro uživatele toochoose hello předplatné, skupinu prostředků a umístění pro nasazení hello. Obecně platí by měly prvky, které se dotázat na úrovni nasazení parametry, jako hello název clusteru nebo správce přihlašovacích údajů, přejděte v tomto kroku.

Pokud elementu chování závisí na předplatné hello uživatele, skupinu prostředků nebo umístění, nelze v základy použít daný element. Například **Microsoft.Compute.SizeSelector** závisí na hello uživatele předplatném a umístění toodetermine hello seznam dostupných velikostí. Proto **Microsoft.Compute.SizeSelector** lze použít pouze v krocích. Obecně platí, pouze elementy v hello **Microsoft.Common** oboru názvů lze použít v základní informace. I když některé prvky v jiných oborech názvů (jako je **Microsoft.Compute.Credentials**), nemáte jsou závislé na kontextu hello uživatele, jsou stále povoleny.

## <a name="steps"></a>Kroky
Vlastnost kroky Hello může obsahovat nula nebo více toodisplay další kroky po základy, z nichž každý obsahuje jeden či více elementů. Zvažte přidání kroků / role nebo vrstvě aplikace hello nasazuje. Například přidáte krok pro vstupy pro hello hlavní uzly a krok pro hello uzly pracovního procesu v clusteru.

## <a name="outputs"></a>Výstupy
Hello portál Azure používá hello `outputs` vlastnost toomap elementy z `basics` a `steps` toohello parametry šablony nasazení Azure Resource Manager hello. Hello klíče tohoto slovníku jsou hello názvy parametrů šablony hello a hello hodnoty jsou vlastnosti objektů hello výstup z elementů hello odkazuje.

## <a name="functions"></a>Funkce
Podobně jako příliš[funkce šablon](resource-group-template-functions.md) v Azure Resource Manager (v syntaxi a funkce i), CreateUiDefinition poskytuje funkce pro práci s vstupy a výstupy elementy, stejně jako funkce, jako je například podmíněné příkazy.

## <a name="next-steps"></a>Další kroky
CreateUiDefinition samotné má jednoduché schéma. skutečné hloubka Hello je pochází z všechny elementy hello podporována a funkce, které hello následující dokumenty popisují udivujících podrobně:

- [Elementy](managed-application-createuidefinition-elements.md)
- [Functions](managed-application-createuidefinition-functions.md)

Zde jsou k dispozici aktuální schéma JSON pro CreateUiDefinition: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json. 

Novější verze bude k dispozici v hello stejné umístění. Nahraďte hello `0.1.2-preview` část adresy URL hello a hello `version` hodnotu s identifikátorem verze hello hodláte toouse. identifikátory Hello aktuálně podporované verze jsou `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, a `0.1.2-preview`.
