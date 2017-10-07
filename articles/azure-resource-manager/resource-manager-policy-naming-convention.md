---
title: "zásady aaaAzure prostředků pro zásady vytváření názvů | Microsoft Docs"
description: "Popisuje zásady správce prostředků Azure pro zásady vytváření názvů prostředků."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: c8384b231263fb694aed8b936a953d5c0ca31e71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a>Použít zásady prostředků pro názvy a text.
Toto téma ukazuje několik [zásady prostředků](resource-manager-policy.md) můžete použít tooestablish konvence pojmenování a text. Tyto zásady zajistit konzistenci pro názvy prostředků a hodnoty značky. 

## <a name="set-naming-convention-with-wildcard"></a>Nastavit zásady vytváření názvů se zástupnými znaky
Hello následující příklad ukazuje použití hello zástupný znak, který je podporovaný rozhraním hello **jako** podmínku. Hello podmínku stavy, pokud hello název neshoduje uvedených vzor hello (namePrefix\*nameSuffix) pak hello požadavek je odepřen:

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a>Nastavit zásady vytváření názvů pomocí vzoru

toospecify, že názvy prostředků odpovídá vzorku, použijte hello vyhovují podmínce. Hello následující příklad vyžaduje toostart názvy s `contoso` a obsahovat šesti další písmena:

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a>Nastavit datum vzor pro hodnotu značky

toorequire datum vzorec dvě číslice, pomlčky, tři písmena, dash a čtyři číslice, použijte:

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a>Další kroky
* Po definování zásad pravidlo (jak je znázorněno v předchozích příkladech hello), budete potřebovat definice zásady hello toocreate a přiřaďte ho tooa oboru. obor Hello může být předplatné, skupinu prostředků nebo prostředek. v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md). zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md). 
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

