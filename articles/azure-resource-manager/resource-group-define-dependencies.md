---
title: "aaaSet pořadím nasazení pro prostředky Azure | Microsoft Docs"
description: "Popisuje, jak se tooset jeden prostředek jako závislé na jiný prostředek během nasazení tooensure prostředky nasadí ve správném pořadí hello."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
ms.openlocfilehash: 2f658f4c85236966c46b34a65aafb8426c92806c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="define-hello-order-for-deploying-resources-in-azure-resource-manager-templates"></a>Definování hello pořadí pro nasazení prostředků v šablonách Azure Resource Manager
Pro daný prostředek může být další prostředky, které musí existovat před nasazením hello prostředků. Například SQL server, musí existovat před pokusem o toodeploy databázi SQL. Můžete definovat tuto relaci označením jeden prostředek jako závislé na hello jiný prostředek. Definování závislostí s hello **dependsOn** element, nebo pomocí hello **odkaz** funkce. 

Správce prostředků vyhodnotí hello závislosti mezi prostředky a nasadí je v pořadí podle jejich závislé. Pokud nejsou na sobě navzájem závislé prostředky, Resource Manager je nasadí současně. Potřebujete jenom toodefine závislosti pro prostředky, které jsou nasazeny v hello stejné šablony. 

## <a name="dependson"></a>dependsOn
V rámci vaší šablony hello dependsOn prvek vám umožní toodefine jeden prostředek jako závisí na jeden nebo více prostředků. Jeho hodnota může být čárkami oddělený seznam názvy prostředků. 

Hello následující příklad ukazuje škálovací sadu virtuálních počítačů, které závisí na Vyrovnávání zatížení, virtuální sítě a smyčku, která vytváří více účtů úložiště. Tyto další prostředky nejsou zobrazeny v hello následující příklad, ale budou potřebovat tooexist jinde v šabloně hello.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

V předchozím příkladu hello, je zahrnuta závislost na hello prostředky, které jsou vytvořené pomocí kopírovací smyčkou s názvem **storageLoop**. Příklad, naleznete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).

Při definování závislostí, můžete použít hello prostředků zprostředkovatele oboru názvů a prostředek typu tooavoid nejednoznačnosti. Například tooclarify, které nástroj pro vyrovnávání zatížení a virtuální síť, která může mít hello stejné názvy jako jiné prostředky, hello použijte následující formát:

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

Může být nakloněné toouse dependsOn toomap vztahy mezi prostředky, je důležité toounderstand proč vaše změny. Pro příklad, jak vzájemně propojeny prostředky, toodocument dependsOn není hello správný přístup. Nemůže zadat dotaz, které prostředky byly definovány v elementu dependsOn hello po nasazení. Pomocí dependsOn můžete případně ovlivnit času nasazení protože Resource Manager není nasazen v paralelní dva prostředky, které jsou závislé. toodocument vztahy mezi prostředky, použijte [propojování prostředků](/rest/api/resources/resourcelinks).

## <a name="child-resources"></a>Podřízené prostředky
Vlastnost Hello prostředky vám umožní toospecify podřízené prostředky, které jsou prostředků související toohello definovaný. Podřízené prostředky se dají jenom definované pěti úrovněmi. Je důležité vytvořit toonote, který není implicitní závislost mezi podřízených prostředků a hello nadřazený prostředek. Pokud třeba hello podřízených prostředků toobe nenasadí po hello nadřazený prostředek, musí explicitně stavu této závislosti s vlastností dependsOn hello. 

Každý nadřazený prostředek akceptuje pouze určité typy prostředků jako podřízené prostředky. Hello přijata typy prostředků jsou určené v hello [schéma šablony](https://github.com/Azure/azure-resource-manager-schemas) hello nadřazené prostředku. Hello název typu prostředku podřízené obsahuje hello název typu prostředku nadřazené hello, jako například **Microsoft.Web/sites/config** a **Microsoft.Web/sites/extensions** jsou obě podřízené prostředky hello  **Microsoft.Web/sites**.

Hello následující příklad ukazuje systému SQL server a databáze SQL. Všimněte si, že je definován explicitní závislosti mezi hello SQL database a SQL server, i když je databáze hello podřízený hello server.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-function"></a>Reference – funkce
Hello [odkazu funkci](resource-group-template-functions-resource.md#reference) umožňuje výrazu tooderive svou hodnotu z jiných dvojice název a hodnota JSON nebo modul runtime prostředky. Odkaz na výrazy implicitně deklarovat, že jeden prostředek závisí na jiném. Obecný formát Hello je:

```json
reference('resourceName').propertyPath
```

V následujícím příkladu hello koncový bod CDN explicitně závisí na hello profil CDN a implicitně závisí na webovou aplikaci.

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

Můžete použít tento element nebo hello dependsOn element toospecify závislosti, ale nepotřebujete toouse i pro hello stejné závislý prostředek. Kdykoli je to možné, použijte implicitní odkaz tooavoid přidání nepotřebné závislostí.

Další, najdete v části toolearn [odkazu funkci](resource-group-template-functions-resource.md#reference).

## <a name="recommendations-for-setting-dependencies"></a>Doporučení pro nastavení závislostí

Při rozhodování, jakou tooset závislosti, použijte následující pokyny hello:

* Nastavte jako několik závislosti míře.
* Nastavte podřízených prostředků jako závislý na prostředku jeho nadřazený.
* Použití hello **odkaz** funkce tooset implicitní závislosti mezi prostředky, které je třeba tooshare vlastnost. Nepřidávejte explicitní závislosti (**dependsOn**) Pokud jste již definována implicitní závislostí. Tento přístup snižuje riziko hello použití nepotřebné závislosti. 
* Nastaví závislost, pokud prostředek nemůže být **vytvořit** bez funkce z jiného prostředku. Nenastavujte závislost, pokud hello prostředků komunikovat po nasazení.
* Umožňují závislosti cascade bez nastavení je explicitně. Například virtuálního počítače závisí na rozhraní virtuální sítě a hello virtuální síťové rozhraní závisí na virtuální síť a veřejné IP adresy. Proto hello virtuální počítač je nasazené po všech tří prostředky, ale nenastavíte explicitně hello virtuálního počítače jako závislé na všechny tři zdroje. Tento postup vysvětluje pořadí závislostí hello a umožňuje jednodušší šablony toochange hello později.
* Pokud hodnota se dá určit před nasazením, zkuste nasazení prostředků hello bez závislosti. Například pokud hodnota konfigurace potřebuje hello název jiného prostředku, nemusí závislost. V tomto návodu nefunguje vždy vzhledem k tomu, že některé prostředky, ověřte existenci hello hello jiný prostředek. Pokud narazíte na chyby, přidejte závislosti. 

Správce prostředků identifikuje cyklické závislosti během ověřování šablony. Pokud se zobrazí chyba oznamující, že existuje cyklická závislost, nevyhodnotí toosee vaší šablony, pokud nejsou potřebné žádné závislosti a lze odebrat. Pokud odebrání závislostí nefunguje, se můžete vyhnout cyklické závislosti přesunutím některé operace nasazení do podřízené prostředky, které jsou nasazeny po hello prostředky, které mají hello cyklická závislost. Předpokládejme například, nasazujete dva virtuální počítače, ale je nutné nastavit na každé z nich najdete toohello jiné vlastnosti. Můžete je nasadit v hello následující pořadí:

1. vm1
2. virtuálního počítače 2
3. Rozšíření na vm1 závisí na vm1 a virtuálního počítače 2. rozšíření Hello nastaví hodnoty vm1, který získá z virtuálního počítače 2.
4. Rozšíření na virtuálního počítače 2 závisí na vm1 a virtuálního počítače 2. rozšíření Hello nastaví hodnoty virtuálního počítače 2, který získá ze vm1.

Informace o vyhodnocování hello pořadí nasazení a řešení chyb při závislostí najdete v tématu [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).

## <a name="next-steps"></a>Další kroky
* toolearn o řešení potíží s závislosti při nasazení, najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* toolearn o vytváření šablon Azure Resource Manageru, najdete v části [vytváření šablon](resource-group-authoring-templates.md). 
* Seznam dostupných funkcí hello v šabloně, naleznete v části [funkce šablon](resource-group-template-functions.md).

