---
title: "Struktura šablony Azure Resource Manager a syntaxe | Microsoft Docs"
description: "Popisuje strukturu a vlastnosti šablon Azure Resource Manager pomocí deklarativní syntaxe JSON."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2017
ms.author: tomfitz
ms.openlocfilehash: b0bc5abd768be0fa5876aaef108cd71a15d94510
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Pochopit strukturu a syntaxe šablon Azure Resource Manager
Tento článek popisuje strukturu šablony Azure Resource Manager. Představuje různé části šablony a vlastnosti, které jsou k dispozici v těchto částech. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnot pro vaše nasazení. Podrobný kurz k vytvoření šablony, najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).

## <a name="template-format"></a>Formát šablony
Ve své nejjednodušší struktuře šablonu obsahuje následující prvky:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Název elementu | Požaduje se | Popis |
|:--- |:--- |:--- |
| $schema |Ano |Umístění souboru schématu JSON, který popisuje verzi jazyka šablony. Použijte adresu URL v předchozím příkladu. |
| contentVersion |Ano |Verze šablony (jako je například 1.0.0.0). Můžete zadat jakoukoli hodnotu pro tento element. Při nasazení prostředků pomocí šablony, tuto hodnotu lze zajistit, aby se používal správnou šablonu. |
| parameters |Ne |Hodnoty, které jsou k dispozici při nasazení pro přizpůsobení nasazení prostředků. |
| proměnné |Ne |Hodnoty, které se používá jako fragmenty JSON v šabloně, které zjednodušují výrazy jazyka šablony. |
| Prostředky |Ano |Typy prostředků, které jsou nasazené nebo aktualizovány v skupinu prostředků. |
| Výstupy |Ne |Hodnoty, které se vrátí po nasazení. |

Každý prvek obsahuje vlastnosti, které můžete zadat. Následující příklad obsahuje úplnou syntaxí šablony:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-the parameter>" 
            }
        }
    },
    "variables": {
        "<variable-name>": "<variable-value>",
        "<variable-object-name>": {
            <variable-complex-type-value>
        },
        "<variable-object-name>": {
            "copy": [
                {
                    "name": "<name-of-array-property>",
                    "count": <number-of-iterations>,
                    "input": {
                        <properties-to-repeat>
                    }
                }
            ]
        },
        "copy": [
            {
                "name": "<variable-array-name>",
                "count": <number-of-iterations>,
                "input": {
                    <properties-to-repeat>
                }
            }
        ]
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Tento článek popisuje části šablony podrobněji.

## <a name="syntax"></a>Syntaxe
Základní syntaxe šablony je JSON. K dispozici v rámci šablony JSON hodnoty rozšířit však výrazy a funkce.  Výrazy jsou zapsané v JSON textové literály jejichž první a poslední znaky jsou hranaté závorky: `[` a `]`, v uvedeném pořadí. Hodnota výrazu vyhodnotí při nasazení šablony. Při zápisu jako řetězcový literál, může být výsledkem vyhodnocení výrazu jiného typu formátu JSON, jako je například pole nebo celé číslo, v závislosti na skutečný výraz.  Tak, aby měl řetězcový literál začínat závorky `[`, ale je interpretován jako výraz, můžete přidat další znak pravé závorky zahájíte řetězec s `[[`.

Obvykle používají výrazy s funkcí k provádění operací pro konfiguraci nasazení. Jenom jako v jazyce JavaScript, volání funkce jsou formátovány jako `functionName(arg1,arg2,arg3)`. Vlastnosti odkazovat pomocí operátorů dot a [index].

Následující příklad ukazuje, jak použít několik funkcí, při vytváření hodnotu:

```json
"variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
}
```

Úplný seznam funkcí šablony najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametry
V sekci parametrů šablony zadejte hodnoty, které můžete zadat při nasazování prostředky. Tyto hodnoty parametrů umožňují přizpůsobit nasazení zadáním hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním). Není nutné zadat parametry v šabloně, ale bez parametrů šablony vždy nasazení stejné prostředky se stejnými názvy, umístění a vlastnosti.

Následující příklad ukazuje definici jednoduchého parametr:

```json
"parameters": {
  "siteNamePrefix": {
    "type": "string",
    "metadata": {
      "description": "The name prefix of the web app that you wish to create."
    }
  },
},
```

Informace o definování parametrů najdete v tématu [oddílu parametry šablon Azure Resource Manager](resource-manager-templates-parameters.md).

## <a name="variables"></a>Proměnné
V sekci proměnných můžete vytvořit hodnoty, které lze použít v celé vaší šablony. Není potřeba definovat proměnné, ale jejich často zjednodušit vaše šablony snížením složité výrazy.

Následující příklad ukazuje definici jednoduché proměnné:

```json
"variables": {
  "webSiteName": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
},
```

Informace o definování proměnné najdete v tématu [části proměnných šablon Azure Resource Manager](resource-manager-templates-variables.md).

## <a name="resources"></a>Zdroje
V části prostředky definujete prostředky, které jsou nasazené a aktualizovat. V této části můžete získat složité, protože je potřeba pochopit, typy, které nasazujete zadejte správné hodnoty.

```json
"resources": [
  {
    "apiVersion": "2016-08-01",
    "name": "[variables('webSiteName')]",
    "type": "Microsoft.Web/sites",
    "location": "[resourceGroup().location]",
    "properties": {
      "serverFarmId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Web/serverFarms/<plan-name>"
    }
  }
],
```

Další informace najdete v tématu [oddílu prostředků šablon Azure Resource Manager](resource-manager-templates-resources.md).

## <a name="outputs"></a>Výstupy
V části výstupy zadejte hodnoty, které jsou vráceny z nasazení. Například může vrátit identifikátor URI pro přístup k prostředkům nasazené.

```json
"outputs": {
  "newHostName": {
    "type": "string",
    "value": "[reference(variables('webSiteName')).defaultHostName]"
  }
}
```

Další informace najdete v tématu [výstupy části šablon Azure Resource Manager](resource-manager-templates-outputs.md).

## <a name="template-limits"></a>Omezení šablony

Omezení velikosti vaší šablony 1 MB a každý soubor parametrů na 64 KB. Omezení 1 MB platí pro konečného stavu šablony po rozšířila s definic iterativní prostředků a hodnoty pro parametry a proměnné. 

Také jste omezeni na:

* 256 parametry
* 256 proměnné
* 800 prostředky (včetně počet kopií)
* 64 výstupní hodnoty
* 24,576 znaků výraz šablony

Některá omezení šablony můžete překročit pomocí vnořené šablony. Další informace najdete v tématu [použití propojených šablon při nasazování prostředků Azure](resource-group-linked-templates.md). Abyste snížili počet parametrů, proměnné nebo výstupů, můžete sloučit několik hodnot do objektu. Další informace najdete v tématu [objektů jako parametry](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Další kroky
* Hotové šablony pro mnoho různých typů řešení najdete na stránce [Šablony Azure pro rychlý start](https://azure.microsoft.com/documentation/templates/).
* Podrobnosti o funkcích, které můžete použít z v rámci šablon najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md).
* Pokud chcete kombinovat několik šablon během nasazení, přečtěte si téma [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* Musíte používat prostředky, které existují v jiné skupině prostředků. Tento scénář je běžný, při práci s účty úložiště a virtuální sítě, které jsou sdíleny více skupin prostředků. Další informace najdete v tématu [resourceId funkce](resource-group-template-functions-resource.md#resourceid).
