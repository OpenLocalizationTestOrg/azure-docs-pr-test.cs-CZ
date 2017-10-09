---
title: "zásady prostředků aaaAzure | Microsoft Docs"
description: "Popisuje, jak se vlastností konzistentní prostředku tooensure zásady Azure Resource Manager toouse nastaví během nasazení. Zásady můžete použít na hello předplatné nebo prostředek skupiny."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: tomfitz
ms.openlocfilehash: f1b0bbb5f838f6bb70721e1040ad3eac2d881cea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-policy-overview"></a>Přehled zásad prostředků
Zásady prostředků povolit tooestablish konvence pro prostředky ve vaší organizaci. Definováním konvence můžete řídit náklady a snadněji spravovat vaše prostředky. Například můžete zadat, že jsou povoleny pouze určité typy virtuálních počítačů. Nebo můžete vyžadovat, aby všechny prostředky měli konkrétní značku. Zásady jsou zdědí všechny podřízené prostředky. Proto pokud je zásada použitá tooa skupinu prostředků, je použít tooall hello prostředky v příslušné skupině prostředků.

Existují dvě toounderstand koncepty týkající se zásad:

* Definice zásad - popisují při hello zásady se vynucují a jaké akce tootake
* přiřazení zásady - použijete hello zásady definice tooa oboru (předplatné nebo skupinu prostředků)

Toto téma se zaměřuje na definice zásady. Informace o přiřazení zásad najdete v tématu [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md) nebo [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).

Při vytváření nebo aktualizaci prostředků (PUT a oprava operations), se vyhodnocují zásady.

> [!NOTE]
> Zásady v současné době nelze vyhodnotit typů prostředků, které nepodporují značky, typ a umístění, jako je například typ prostředku Microsoft.Resources/deployments hello. Tato podpora se přidá na datum v budoucnosti. problémy s kompatibilitou zpětné tooavoid, byste měli explicitně zadat typ při vytváření zásad. Například značky zásadu, která neurčuje typy platí pro všechny typy. V takovém případě šablony nasazení může selhat, pokud je vnořeného prostředku, který nepodporuje značky a typ prostředku nasazení hello přidala toopolicy vyhodnocení. 
> 
> 

## <a name="how-is-it-different-from-rbac"></a>Jak se liší od RBAC?
Existuje několik hlavní rozdíly mezi zásadami a řízení přístupu na základě role (RBAC). RBAC se zaměřuje na **uživatele** akce na různých místech. Například se přidají toohello role Přispěvatel pro skupinu prostředků v hello požadovaného oboru, umožní vám provádět změny toothat prostředků skupiny. Zásady se zaměřuje na **prostředků** vlastnosti během nasazení. Například prostřednictvím zásad, můžete řídit hello typů prostředků, které mohou být zřízeny. Nebo můžete omezit hello umístění, ve kterých se dá zřídit prostředky hello. Na rozdíl od RBAC, je povolit výchozí zásady a explicitní odepřít systému. 

toouse zásady, které musí být ověřeny pomocí RBAC. Konkrétně, musí váš účet:

* `Microsoft.Authorization/policydefinitions/write`oprávnění toodefine zásadu
* `Microsoft.Authorization/policyassignments/write`oprávnění tooassign zásadu 

Tato oprávnění nejsou součástí hello **Přispěvatel** role.

## <a name="built-in-policies"></a>Předdefinované zásady

Azure poskytuje definice předdefinované zásady, které může snížit počet hello zásad máte toodefine. Před pokračováním definice zásady, byste měli zvážit, zda předdefinovaných zásad již poskytuje hello definice, které potřebujete. definice Hello předdefinovaných zásad jsou:

* Povolených umístění
* Typy povolené prostředků
* Povolené účet úložiště SKU
* Povolená SKU virtuálního počítače
* Použít značky a výchozí hodnota
* Vynutit značky a hodnota
* Není povoleno typy prostředků
* Vyžaduje systém SQL Server verze 12.0
* Vyžadovat šifrování účtu úložiště

Můžete přiřadit některé z těchto zásad prostřednictvím hello [portál](resource-manager-policy-portal.md), [prostředí PowerShell](resource-manager-policy-create-assign.md#powershell), nebo [rozhraní příkazového řádku Azure](resource-manager-policy-create-assign.md#azure-cli).

## <a name="policy-definition-structure"></a>Definice strukturu zásad.
Používáte JSON toocreate definici zásady. Definice zásad Hello obsahuje prvky pro:

* parameters
* Zobrazovaný název
* description
* Pravidlo zásad
  * logické vyhodnocení
  * Platnost

Hello následující příklad ukazuje zásadu, která omezuje, kde jsou nasazené prostředky:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="parameters"></a>Parametry
Pomocí parametrů pomáhá zjednodušit vaší zásady správy, protože snižuje počet hello definice zásady. Můžete definovat zásady pro vlastnost prostředku (například omezení hello umístění, kde můžete nasadit prostředky) a zahrnout parametry v definici hello. Pak můžete znovu použít, aby definice zásady pro různé scénáře předávání různé hodnoty (například určení jednu sadu umístění pro předplatné) při přiřazování zásady hello.

Parametry deklarovat, při vytváření definice zásad.

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "hello list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

Hello typ parametru může být řetězec nebo pole. Vlastnost metadat Hello se používá pro nástroje, například Azure portálu toodisplay uživatelské informace. 

Pravidlo zásad hello odkazujete parametry s hello následující syntaxi: 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Zobrazovaný název a popis

Použít hello **displayName** a **popis** tooidentify hello definice zásady a poskytují kontext pro při použití.

## <a name="policy-rule"></a>Pravidlo zásad

pravidlo zásad Hello se skládá z **Pokud** a **pak** bloky. V hello **Pokud** blok, můžete definovat jednu nebo víc podmínek, které určují, kdy hello zásady se vynucují. Logické operátory toothese podmínky lze použít tooprecisely definovat hello scénář pro zásady. V hello **pak** blok, můžete definovat hello vliv, který se stane, když hello **Pokud** jsou splněny podmínky.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Logické operátory
logické operátory Hello podporována jsou:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

Hello **není** syntaxe Invertuje výběr hello výsledek hello podmínku. Hello **allOf** syntaxe (podobně jako toohello logické **a** operaci) vyžaduje všechny podmínky toobe true. Hello **anyOf** syntaxe (podobně jako toohello logické **nebo** operaci) vyžaduje jeden nebo více podmínek toobe true.

Logické operátory lze vnořit. Následující příklad ukazuje Hello **není** operace, která je vnořen do **allOf** operaci. 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Podmínky
Hello podmínka vyhodnocena jako jestli **pole** splňuje určitá kritéria. jsou podporovány Hello podmínky:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Při použití hello **jako** podmínku, můžete zadat zástupný znak (*) v hodnotě hello.

Při použití hello **odpovídat** podmínky, zadejte `#` toorepresent číslici, `?` pro písmeno a všechny další znak toorepresent tento skutečný znak. Příklady najdete v tématu [prostředků pomocí zásad pro názvy a text](resource-manager-policy-naming-convention.md).

### <a name="fields"></a>Pole
Podmínky se vytváří pomocí pole. Pole představuje vlastnosti v datová část požadavku prostředků hello tedy stav hello použité toodescribe hello prostředku.  

jsou podporovány Hello následující pole:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* Vlastnost aliasy – seznam najdete v tématu [aliasy](#aliases).

### <a name="effect"></a>Efekt
Zásady podporuje tři typy efekt – `deny`, `audit`, a `append`. 

* **Odepřít** vygeneruje událost v protokolu auditu hello a selže hello požadavku
* **Audit** vygeneruje událost upozornění v protokolu auditu ale neselže hello požadavku
* **Připojit** přidá hello definované sadu pole toohello požadavku 

Pro **připojit**, je nutné zadat hello následující podrobnosti:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of hello field"
  }
]
```

Hello hodnota může být řetězec nebo objekt formátu JSON. 

## <a name="aliases"></a>Aliasy

Vlastnost aliasy tooaccess specifické vlastnosti se používá pro typ prostředku. Aliasy umožňují toorestrict jsou povoleny jaké hodnoty nebo podmínky pro vlastnost prostředku. Každý alias mapuje toopaths v různých verzích rozhraní API pro typ daného prostředku. Během hodnocení zásad získá modul zásad hello hello vlastnost cesty pro tuto verzi rozhraní API.

**Microsoft.Cache/Redis**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Jestli port serveru Redis bez ssl hello (6379) je povoleno nastavení. |
| Microsoft.Cache/Redis/shardCount | Nastavte hello počet horizontálních oddílů toobe vytvořen na clusteru Cache ve verzi Premium.  |
| Microsoft.Cache/Redis/sku.capacity | Nastavit velikost hello toodeploy mezipaměti Redis hello.  |
| Microsoft.Cache/Redis/sku.family | Nastavit rodiny toouse hello SKU. |
| Microsoft.Cache/Redis/sku.name | Nastavte typ hello toodeploy Redis Cache. |

**Microsoft.Cdn/profiles**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | Nastavte název hello hello cenová úroveň. |

**Microsoft.Compute/disks**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Sada hello nabídku marketplace obrázku nebo image platformy hello používá toocreate hello virtuálního počítače. |
| Microsoft.Compute/imagePublisher | Nastavit hello vydavatele hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/imageSku | Nastavte hello SKU hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/imageVersion | Verze sady hello image platformy hello nebo marketplace image používat toocreate hello virtuálního počítače. |


**Microsoft.Compute/virtualMachines**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Compute/imageId | Nastavit identifikátor hello hello bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/imageOffer | Sada hello nabídku marketplace obrázku nebo image platformy hello používá toocreate hello virtuálního počítače. |
| Microsoft.Compute/imagePublisher | Nastavit hello vydavatele hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/imageSku | Nastavte hello SKU hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/imageVersion | Verze sady hello image platformy hello nebo marketplace image používat toocreate hello virtuálního počítače. |
| Microsoft.Compute/licenseType | Nastavení této bitové kopie hello nebo disk je licencovaný místní. Tato hodnota se používá pouze pro bitové kopie, které obsahují operační systém Windows Server hello.  |
| Microsoft.Compute/virtualMachines/imageOffer | Sada hello nabídku marketplace obrázku nebo image platformy hello používá toocreate hello virtuálního počítače. |
| Microsoft.Compute/virtualMachines/imagePublisher | Nastavit hello vydavatele hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/virtualMachines/imageSku | Nastavte hello SKU hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/virtualMachines/imageVersion | Verze sady hello image platformy hello nebo marketplace image používat toocreate hello virtuálního počítače. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Nastavte hello vhd identifikátor URI. |
| Microsoft.Compute/virtualMachines/sku.name | Nastavení velikosti hello hello virtuálního počítače. |

**Microsoft.Compute/virtualMachines/extensions**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Název sady hello hello rozšíření vydavatele. |
| Microsoft.Compute/virtualMachines/extensions/type | Nastavit hello typ rozšíření. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Nastavit verzi hello hello rozšíření. |

**Microsoft.Compute/virtualMachineScaleSets**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Compute/imageId | Nastavit identifikátor hello hello bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/imageOffer | Sada hello nabídku marketplace obrázku nebo image platformy hello používá toocreate hello virtuálního počítače. |
| Microsoft.Compute/imagePublisher | Nastavit hello vydavatele hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/imageSku | Nastavte hello SKU hello platformy bitovou kopii nebo marketplace bitová kopie používaná toocreate hello virtuálního počítače. |
| Microsoft.Compute/imageVersion | Verze sady hello image platformy hello nebo marketplace image používat toocreate hello virtuálního počítače. |
| Microsoft.Compute/licenseType | Nastavení této bitové kopie hello nebo disk je licencovaný místní. Tato hodnota se používá pouze pro bitové kopie, které obsahují operační systém Windows Server hello. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Nastavit hello předpony názvu počítače pro všechny virtuální počítače hello v sadě škálování hello. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Nastavte hello identifikátor URI objektu blob bitové kopie uživatele. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | Nastavit adresy URL hello kontejneru, které jsou použité toostore disky operačního systému pro sadu škálování hello. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Nastavení velikosti hello virtuálních počítačů v sadě škálování. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Nastavit úroveň hello virtuálních počítačů v sadě škálování. |
  
**Microsoft.Network/applicationGateways**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Nastavení velikosti hello hello brány. |

**Microsoft.Network/virtualNetworkGateways**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Nastavte typ hello tuto bránu virtuální sítě. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Nastavte název SKU brány hello. |

**Microsoft.Sql/servers**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Nastavit hello verzi serveru hello. |

**Microsoft.Sql/databases**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Nastavit edici hello hello databáze. |
| Microsoft.Sql/servers/databases/elasticPoolName | Název sady hello hello elastického fondu hello databáze je v. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Nastavit hello nakonfigurované služby cíle ID úrovně hello databáze. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Název sady hello hello nakonfigurované cíle na úrovni služby databáze hello.  |

**Microsoft.Sql/elasticpools**

| Alias | Popis |
| ----- | ----------- |
| servery nebo elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Sada hello celkem sdílené DTU fondu elastické databáze hello. |
| servery nebo elasticpools | Microsoft.Sql/servers/elasticPools/edition | Nastavit edici hello hello elastického fondu. |

**Microsoft.Storage/storageAccounts.**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Úroveň přístupu hello sady používané pro fakturaci. |
| Microsoft.Storage/storageAccounts/accountType | Název SKU hello sady. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Nastavte, jestli služba hello šifruje hello data, jak je uložen v úložišti služby objektů blob hello. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Nastavte, jestli služba hello šifruje hello data, jak je uložen v úložišti služby hello souboru. |
| Microsoft.Storage/storageAccounts/sku.name | Název SKU hello sady. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Nastavení tooallow pouze https služby toostorage provoz. |


## <a name="policy-examples"></a>Příklady zásad

Hello následující témata obsahují příklady zásad:

* Příklady značky zásady, najdete v části [zásad prostředků pro značky](resource-manager-policy-tags.md).
* Příklady vzory pojmenovávání a text najdete v tématu [prostředků pomocí zásad pro názvy a text](resource-manager-policy-naming-convention.md).
* Příklady úložiště zásad najdete v tématu [použít účty toostorage zásady prostředků](resource-manager-policy-storage.md).
* Příklady zásad virtuálního počítače najdete v tématu [použít zásady tooLinux prostředků virtuálních počítačů](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) a [použít zásady tooWindows prostředků virtuálních počítačů](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)


## <a name="next-steps"></a>Další kroky
* Po definování pravidla zásad, přiřaďte ji tooa oboru. v tématu Zásady tooassign prostřednictvím portálu hello [Azure pomocí portálu tooassign a spravovat zásady prostředků](resource-manager-policy-portal.md). zásady tooassign prostřednictvím REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).
* schéma zásad Hello je publikována v [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

