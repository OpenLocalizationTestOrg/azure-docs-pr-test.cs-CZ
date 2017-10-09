---
title: "aaaAzure Resource Manager strukturu šablony a syntaxe | Microsoft Docs"
description: "Popisuje hello strukturu a vlastnosti šablon Azure Resource Manager pomocí deklarativní syntaxe JSON."
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
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: b0709852f8777c91cc1704d6bca16257a017d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-structure-and-syntax-of-azure-resource-manager-templates"></a>Pochopit strukturu hello a syntaxe šablon Azure Resource Manager
Toto téma popisuje strukturu hello šablony Azure Resource Manager. Představuje různé části šablony a hello vlastnosti, které jsou k dispozici v těchto částech hello. Šablona Hello se skládá z JSON a výrazy, můžete použít hodnoty tooconstruct pro vaše nasazení. Podrobný kurz k vytvoření šablony, najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).

## <a name="template-format"></a>Formát šablony
Ve své nejjednodušší struktura obsahuje šablonu hello následující prvky:

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
| $schema |Ano |Umístění souboru schématu JSON hello, který popisuje hello verze jazyka šablony hello. Použijte adresu URL hello uvedené v předchozím příkladu hello. |
| contentVersion |Ano |Verze šablony hello (například 1.0.0.0). Můžete zadat jakoukoli hodnotu pro tento element. Při nasazení prostředků pomocí šablony hello, tato hodnota může být použité toomake se, že šablona správné hello používá. |
| parameters |Ne |Hodnoty, které jsou k dispozici po nasazení provést nasazení toocustomize prostředků. |
| proměnné |Ne |Hodnoty, které jsou použity jako fragmenty JSON ve výrazech jazyka šablony toosimplify šablony hello. |
| Prostředky |Ano |Typy prostředků, které jsou nasazené nebo aktualizovány v skupinu prostředků. |
| Výstupy |Ne |Hodnoty, které se vrátí po nasazení. |

Každý prvek obsahuje vlastnosti, které můžete zadat. Následující ukázka Hello obsahuje hello úplnou syntaxí šablony:

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
                "description": "<description-of-hello parameter>" 
            }
        }
    },
    "variables": {  
        "<variable-name>": "<variable-value>",
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
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

Jsme zkontrolujte hello části šablony hello podrobněji později v tomto tématu.

## <a name="expressions-and-functions"></a>Výrazy a funkce
Základní syntaxe Hello hello šablony je JSON. Výrazy a funkce však rozšířit hodnoty na JSON hello, které jsou k dispozici v rámci šablony hello.  Výrazy jsou zapsané v JSON textové literály jejichž první a poslední znaky jsou hello závorky: `[` a `]`, v uvedeném pořadí. Hello hodnotu výrazu hello vyhodnotí při nasazení šablony hello. Při zápisu jako řetězcový literál, může být výsledkem vyhodnocení výrazu hello hello jiného typu formátu JSON, jako je například pole nebo celé číslo, v závislosti na skutečný výraz hello.  řetězcový literál začínat závorky toohave `[`, ale je interpretován jako výraz, můžete přidat řetězec navíc závorky toostart hello s `[[`.

Výrazy se obvykle používá s operacemi tooperform funkce pro konfiguraci nasazení hello. Jenom jako v jazyce JavaScript, volání funkce jsou formátovány jako `functionName(arg1,arg2,arg3)`. Vlastnosti odkazovat pomocí operátorů dot a [index] hello.

Hello následující příklad ukazuje, jak toouse několik funkcí při vytváření hodnoty:

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

Hello úplný seznam funkcí šablony najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametry
V části Parametry hello hello šablony zadejte hodnoty, které můžete zadat při nasazování hello prostředky. Tyto hodnoty parametrů povolit nasazení hello toocustomize zadáním hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním). Nemáte tooprovide parametry v šabloně, ale bez parametrů šablony vždy nasazení hello stejné prostředky s hello stejné názvy a umístění a vlastností.

Definujte parametry s hello strukturu:

```json
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
            "description": "<description-of-hello parameter>" 
        }
    }
}
```

| Název elementu | Požaduje se | Popis |
|:--- |:--- |:--- |
| Název parametru |Ano |Název parametru hello. Musí být platný identifikátor jazyka JavaScript. |
| type |Ano |Typ hodnoty parametru hello. Zobrazit hello seznam povolených typů za touto tabulkou. |
| Výchozí hodnota |Ne |Výchozí hodnota pro parametr hello, pokud není zadána žádná hodnota pro parametr hello. |
| allowedValues |Ne |Pole povolených hodnot pro jistotu, že je zadaná hodnota pravé hello toomake parametr hello. |
| MinValue |Ne |Hello minimální hodnotu pro parametry typ int, tato hodnota je (včetně). |
| MaxValue |Ne |Hello maximální hodnotu pro parametry typ int, tato hodnota je (včetně). |
| minLength |Ne |Minimální délka Hello string, secureString a parametry typu pole, tato hodnota je (včetně). |
| Hodnota maxLength |Ne |Maximální délka Hello string, secureString a parametry typu pole, tato hodnota je (včetně). |
| description |Ne |Zobrazí popis hello parametr, který je toousers prostřednictvím portálu hello. |

Hello povolené typy a hodnoty jsou:

* **řetězec**
* **secureString**
* **celá čísla**
* **BOOL**
* **objekt** 
* **secureObject**
* **pole**

toospecify parametr jako volitelná, zadejte hodnotu defaultValue (může být prázdný řetězec). 

Pokud zadáte název parametru v šabloně odpovídající parametr v hello příkaz toodeploy hello šablony, je potenciální nejednoznačnosti hello hodnoty, které zadáte. Správce prostředků řeší tento nedorozuměním přidáním hello operátory **FromTemplate** toohello parametr šablony. Například, pokud zahrnete parametr s názvem **ResourceGroupName** v šabloně, je v konfliktu s hello **ResourceGroupName** parametr v hello [ Nové AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) rutiny. Během nasazení se výzvami tooprovide hodnotu **ResourceGroupNameFromTemplate**. Obecně platí neměli byste toto nedorozuměním není pojmenováním parametry s hello stejný název jako parametry použité pro operace nasazení.

> [!NOTE]
> Všechna hesla, klíče a jiné tajné měli používat hello **secureString** typu. Pokud předáte citlivá data v objektu JSON, použít hello **secureObject** typu. Parametry šablony s secureString nebo secureObject typů nelze přečíst po nasazení prostředků. 
> 
> Hello následující položku v historii nasazení hello příkladu hello hodnotu pro řetězce a objekt, ale ne pro secureString a secureObject.
>
> ![Zobrazit hodnoty nasazení](./media/resource-group-authoring-templates/show-parameters.png)  
>

Následující příklad ukazuje, jak Hello toodefine parametry:

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

Jak tooinput hello parametr hodnoty během nasazení, naleznete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md). 

## <a name="variables"></a>Proměnné
V části proměnných hello vytvořit hodnoty, které lze použít v celé vaší šablony. Není nutné toodefine proměnné, ale jejich často zjednodušit vaše šablony snížením složité výrazy.

Můžete zadat proměnné určené s hello strukturu:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

Následující příklad ukazuje, jak Hello toodefine proměnné, která se vytvářejí na základě dvě hodnoty parametrů:

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

Hello další příklad ukazuje, proměnné, která je komplexního typu JSON a proměnné, které se vytvářejí na základě jiné proměnné:

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a>Zdroje
V části prostředky hello definujete hello prostředky, které jsou nasazené a aktualizovat. V této části můžete získat složité, protože musíte pochopit hello typy, které nasazujete tooprovide hello správné hodnoty. Hello konkrétní prostředky hodnoty (apiVersion, typ a vlastnosti), je nutné, aby tooset naleznete v části [definování zdrojů v šablonách Azure Resource Manager](/azure/templates/). 

Definování zdrojů s hello strukturu:

```json
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
]
```

| Název elementu | Požaduje se | Popis |
|:--- |:--- |:--- |
| Podmínka | Ne | Logická hodnota, která určuje, jestli je nasazené hello prostředků. |
| apiVersion |Ano |Verze toouse hello REST API pro vytváření prostředků hello. |
| type |Ano |Typ prostředku hello. Tato hodnota je kombinací hello obor názvů zprostředkovatele prostředků hello a typ prostředku hello (například **Microsoft.Storage/storageAccounts**). |
| jméno |Ano |Název prostředku hello. Hello název musí splňovat omezení součást URI definované v RFC3986. Kromě toho služby Azure, které zveřejňují hello prostředků název toooutside strany ověřit název toomake hello, že není pokusu o toospoof jiné identity. |
| location |Je to různé. |Podporovaná geo umístění hello zadaný prostředek. Můžete vybrat libovolný hello dostupných umístění, ale obvykle má smysl toopick ten, který je zavřít tooyour uživatele. Obvykle se také má smysl tooplace prostředky, které vzájemně spolupracovat v hello stejné oblasti. Většina typů prostředků vyžadují umístění, ale některé typy (například přiřazení role) nevyžadují umístění. V tématu [nastavit umístění prostředku v šablonách Azure Resource Manager](resource-manager-template-location.md). |
| tags |Ne |Značky, které jsou přidružené hello prostředků. V tématu [označit prostředky v šablonách Azure Resource Manager](resource-manager-template-tags.md). |
| Komentáře |Ne |Poznámky pro dokumentaci hello prostředky ve vaší šabloně |
| Kopírování |Ne |V případě potřeby více než jednu instanci hello počet toocreate prostředky. paralelní je výchozí režim Hello. Zadejte sériové režim, když mají všechny nebo z nich hello toodeploy prostředky v hello stejnou dobu. Další informace najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md). |
| dependsOn |Ne |Prostředky, které musí být nasazené, než je nasazený tento prostředek. Správce prostředků vyhodnotí hello závislosti mezi prostředky a nasadí je ve správném pořadí hello. Pokud nejsou na sobě navzájem závislé prostředky, jsou nasazeny současně. Hello hodnota může být čárkami oddělený seznam prostředek názvy nebo jedinečné identifikátory prostředků. Zobrazit seznam pouze těch prostředků, které jsou nasazeny v této šabloně. Prostředky, které nejsou v této šabloně definovány již musí existovat. Vyhněte se přidání nepotřebné závislostí, jak mohou zpomalit nasazení a vytvoření cyklické závislosti. Pokyny v závislosti na nastavení najdete v tématu [definování závislostí v šablonách Azure Resource Manager](resource-group-define-dependencies.md). |
| properties |Ne |Nastavení konfigurace specifických prostředků. Hello hodnoty vlastností hello jsou hello stejné jako hello hodnoty, které zadáte v textu žádosti hello hello REST API operaci (metoda PUT) toocreate hello prostředků. Můžete také určit toocreate pole kopie více instancí vlastnosti. Další informace najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md). |
| Prostředky |Ne |Podřízené prostředky, které jsou závislé na prostředku hello definovaný. Zadejte pouze typy prostředků, které jsou povoleny ve schématu hello hello nadřazené prostředku. Hello plně kvalifikovaný typ prostředku podřízené hello obsahuje hello nadřazený typ prostředku, jako například **Microsoft.Web/sites/extensions**. Závislost na nadřazený prostředek hello není implicitní. Je nutné explicitně zadat tuto závislost. |

Hello prostředků obsahuje pole toodeploy prostředky hello. V rámci každého prostředku můžete také definovat pole podřízené prostředky. Proto vaše oddílu prostředků může mít struktura jako:

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

Další informace o definování podřízené prostředky najdete v tématu [nastavte název a typ pro podřízený prostředek v šabloně Resource Manager](resource-manager-template-child-resource.md).

Hello **podmínku** element určuje, jestli je nasazené hello prostředků. Hello hodnota pro tento element překládá tootrue, nebo hodnotu NEPRAVDA. Například toospecify zda nový účet úložiště je nasazená, použijte:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Příklad použití nový nebo existující prostředek, naleznete v části [nový nebo stávající šablonu podmínku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

toospecify zda virtuální počítač nasazený pomocí hesla nebo klíče SSH, definovat dvě verze hello virtuálního počítače šablony a použít **podmínku** toodifferentiate využití. Předání parametru, který určuje, které toodeploy scénář.

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

Příklad použití hesla nebo SSH klíče toodeploy virtuálního počítače, naleznete v části [uživatelské jméno nebo SSH podmínku šablony](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="outputs"></a>Výstupy
V části výstupy hello zadejte hodnoty, které jsou vráceny z nasazení. Například může vrátit hello URI tooaccess nasazené prostředků.

Hello následující příklad ukazuje hello Struktura definice výstup:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Název elementu | Požaduje se | Popis |
|:--- |:--- |:--- |
| outputName |Ano |Název hodnoty výstup hello. Musí být platný identifikátor jazyka JavaScript. |
| type |Ano |Typ hodnoty výstup hello. Výstupní hodnoty podporovat hello stejné typy jako vstupní parametry šablony. |
| hodnota |Ano |Výraz jazyka šablony, který se vyhodnotí a vrátí jako výstupní hodnotu. |

Hello následující příklad ukazuje hodnotu, která je vrácena v části výstupy hello.

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

Další informace o práci s výstupem najdete v tématu [sdílení stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).

## <a name="template-limits"></a>Omezení šablony

Omezit velikost hello vaše šablona too1 MB a každý parametr souboru too64 KB. omezení 1 MB Hello platí toohello konečného stavu hello šablony po rozšířila s definic iterativní prostředků a hodnoty pro parametry a proměnné. 

Také jste omezeni na:

* 256 parametry
* 256 proměnné
* 800 prostředky (včetně počet kopií)
* 64 výstupní hodnoty
* 24,576 znaků výraz šablony

Některá omezení šablony můžete překročit pomocí vnořené šablony. Další informace najdete v tématu [použití propojených šablon při nasazování prostředků Azure](resource-group-linked-templates.md). číslo hello tooreduce parametry, proměnné nebo výstupů, můžete sloučit několik hodnot do objektu. Další informace najdete v tématu [objektů jako parametry](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Další kroky
* tooview dokončení šablon pro mnoho různých typů řešení, najdete v části hello [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).
* Podrobnosti o funkcích hello můžete použít z v rámci šablon najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md).
* toocombine více šablony během nasazení, viz [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* Může být nutné toouse prostředky, které existují v jiné skupině prostředků. Tento scénář je běžný, při práci s účty úložiště a virtuální sítě, které jsou sdíleny více skupin prostředků. Další informace najdete v tématu hello [resourceId funkce](resource-group-template-functions-resource.md#resourceid).
