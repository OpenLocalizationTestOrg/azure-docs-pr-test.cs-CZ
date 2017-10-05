---
title: "Vytvoření a publikování aplikace spravované katalogu služby Azure | Microsoft Docs"
description: "Ukazuje, jak vytvořit Azure spravované aplikace, která je určena pro členy vaší organizace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 39b74984ec2f89ed39753963de7fe3ff79577c9e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Publikování spravované aplikace pro interní používání

Můžete vytvářet a publikovat Azure [spravované aplikace](managed-application-overview.md) , jsou určené pro členy vaší organizace. IT oddělení například může publikovat spravovaných aplikací, které bylo možné zajistit kompatibilitu s organizační standardy. Tyto spravované aplikace jsou k dispozici prostřednictvím katalogu služeb, není v Azure Marketplace.

Chcete-li publikovat spravované aplikace pro katalogu služeb, postupujte takto:

* Vytvořte balíček ZIP, který obsahuje tři soubory požadované šablony.
* Rozhodněte, které uživatele, skupiny nebo aplikace potřebuje přístup ke skupině prostředků v předplatném uživatele.
* Vytvořte definici spravované aplikace, která odkazuje na balíček ZIP a požaduje přístup pro identitu.

## <a name="create-a-managed-application-package"></a>Vytvořit balíček spravované aplikace

Prvním krokem je vytvoření tři soubory požadované šablony. Všechny tři soubory balíčku do souboru ZIP a nahrajte ho do přístupného umístění, jako je například účet úložiště. Při vytváření definice spravované aplikace předáte odkaz na tento soubor .zip.

* **applianceMainTemplate.json**: Tento soubor definuje prostředky Azure, které jsou zřízené v rámci spravované aplikace. Šablona je nejsou jiné než regulární šablony Resource Manageru. Například pokud chcete vytvořit účet úložiště prostřednictvím spravované aplikace, applianceMainTemplate.json obsahuje:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]",
            "apiVersion": "2016-01-01",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {}
  }
  ```

* **mainTemplate.json**: uživatelé nasadit tuto šablonu při vytváření spravované aplikace. Definuje zdroj spravované aplikace, který je typem Microsoft.Solutions/appliances prostředku. Tento soubor obsahuje všechny parametry, které potřebujete k prostředkům v applianceMainTemplate.json.

  Nastavíte dvě důležité vlastnosti v této šabloně. Nejdřív **applianceDefinitionId** vlastnost je ID definice spravovaných aplikací. Můžete vytvořit definici později v tomto tématu. Při nastavování této hodnoty, musíte rozhodnout, které předplatném nebo skupině prostředků pro ukládání definice spravovaných aplikací. A, musíte se rozhodnout na název pro definici. ID je ve formátu:

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  Druhý, **managedResourceGroupId** vlastnost je ID skupiny prostředků, kde jsou prostředky Azure vytvořeny. Můžete přiřadit hodnotu pro tento název skupiny prostředků, nebo mohl uživatel, zadejte název. Formát ID je:

  `/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.

  Následující příklad ukazuje soubor mainTemplate.json. Určuje skupinu prostředků pro nasazené prostředky. Definici ID nastaven pro použití s názvem definice **storageApp** ve skupině prostředků s názvem **managedApplicationGroup**. Tyto hodnoty použít jiné názvy, můžete změnit. Zadejte vlastní ID předplatného v ID definice.

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        }
    },
    "variables": {
        "managedRGId": "[concat(resourceGroup().id,'-application-resources')]",
        "managedAppName": "[concat('managedStorage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Solutions/appliances",
            "name": "[variables('managedAppName')]",
            "apiVersion": "2016-09-01-preview",
            "location": "[resourceGroup().location]",
            "kind": "ServiceCatalog",
            "properties": {
                "managedResourceGroupId": "[variables('managedRGId')]",
                "applianceDefinitionId": "/subscriptions/<subscription-id>/resourceGroups/managedApplicationGroup/providers/Microsoft.Solutions/applianceDefinitions/storageApp",
                "parameters": {
                    "storageAccountNamePrefix": {
                        "value": "[parameters('storageAccountNamePrefix')]"
                    }
                }
            }
        }
    ]
  }
  ```

* **applianceCreateUiDefinition.json**: portál Azure používá tento soubor ke generování uživatelského rozhraní pro uživatele, kteří vytvářejí spravované aplikace. Můžete definovat, jak uživatelé zadali vstup pro jednotlivé parametry. Možnosti můžete použít jako rozevíracího seznamu, textové pole, pole pro heslo a další vstupní nástroje. Naučte se vytvořit soubor definice uživatelského rozhraní pro spravované aplikace, najdete v tématu [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).

  Následující příklad ukazuje soubor applianceCreateUiDefinition.json, která umožní uživatelům zadat předponu názvu účtu úložiště prostřednictvím textové pole.

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {
                "name": "storageAccounts",
                "type": "Microsoft.Common.TextBox",
                "label": "Storage account name prefix",
                "defaultValue": "storage",
                "toolTip": "Provide a value that is used for the prefix of your storage account. Limit to 11 characters.",
                "constraints": {
                    "required": true,
                    "regex": "^[a-z0-9A-Z]{1,11}$",
                    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-11 characters long."
                },
                "visible": true
            }
        ],
        "steps": [],
        "outputs": {
            "storageAccountNamePrefix": "[basics('storageAccounts')]"
        }
    }
  }
  ```

Po všechny potřebné soubory jsou připravené, jejich zabalení jako soubor ZIP. Tři soubory musí být na kořenové úrovni souboru ZIP. Pokud je vložit do složky, obdržíte chybu při vytváření definice spravovaných aplikací, s oznámením, že nejsou k dispozici požadované soubory. Nahrání balíčku na dostupné místo z kde ji můžete použít. Zbývající část tohoto článku předpokládá, že soubor .zip existuje v kontejneru objektů blob veřejně dostupné úložiště.

## <a name="create-an-azure-active-directory-user-group-or-application"></a>Vytvoření skupiny uživatelů Azure Active Directory nebo aplikace

Druhým krokem je vybrat skupiny uživatelů nebo aplikace pro správu k prostředkům jménem zákazníka. Této skupiny uživatelů nebo aplikací má oprávnění pro skupinu spravovaných prostředků podle role, která je přiřazena. Tato role může být žádné předdefinované role řízení přístupu na základě Role (RBAC) jako vlastníka nebo přispěvatele. Také můžete udělit oprávnění jednotlivého uživatele ke správě prostředků, ale obvykle přiřadit toto oprávnění pro skupinu uživatelů. Chcete-li vytvořit novou skupinu uživatelů služby Active Directory, přečtěte si téma [vytvořte skupinu a přidejte členy v Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

Je třeba ID objektu skupiny uživatelů pro řízení zdrojů. Následující příklad ukazuje, jak získat ID objektu ze skupiny zobrazovaný název:

```azurecli-interactive
az ad group show --group exampleGroupName
```

V ukázkovém příkazu vrátí následující výstup:

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

Pro načtení ID objektu, použijte:

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-the-role-definition-id"></a>Získání ID definice role

Dále je nutné zadat ID definice role RBAC předdefinovaná role, které chcete udělit přístup pro uživatele, skupiny uživatelů nebo aplikací. Obvykle použijete roli vlastníka nebo přispěvatele nebo čtečky. Následující příkaz ukazuje, jak získat ID definice role pro roli vlastníka:


```azurecli-interactive
az role definition list --name owner
```

Tento příkaz vrátí následující výstup:

```azurecli
{
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "properties": {
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything, including access to resources.",
      "permissions": [
        {
          "actions": [
            "*"
         ],
         "notActions": []
        }
      ],
      "roleName": "Owner",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
}
```

Hodnota vlastnosti "název" z předchozího příkladu potřebujete. Můžete získat pouze vlastnosti s:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-the-managed-application-definition"></a>Vytvořit definici spravované aplikace

Pokud již jste skupinu prostředků pro ukládání definice spravované aplikace, vytvořte jeden:

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

Teď vytvořte prostředek definice spravované aplikace.

```azurecli-interactive
az managedapp definition create \
  --name storageApp \
  --location "westcentralus" \
  --resource-group managedApplicationGroup \
  --lock-level ReadOnly \
  --display-name myteststorageapp \
  --description storageapp \
  --authorizations "$groupid:$roleid" \
  --package-file-uri <uri-path-to-zip-file>
```

Parametry použité v předchozím příkladu jsou:

* **Skupina prostředků**: název skupiny prostředků, kde se má vytvořit definici spravované aplikace.
* **Úroveň zámku**: typ zámku umístit do skupiny spravovaných prostředků. Zákazník brání provádění nežádoucího operací v této skupině prostředků. V současné době určené jen pro čtení je pouze podporovanou úroveň zámku. Pokud je zadána jen pro čtení, Zákazník může číst pouze ve skupině prostředků spravované prostředky.
* **povolení**: Popisuje ID objektu zabezpečení a ID definice role, které se používají k udělení oprávnění ke skupině spravovaných prostředků. Jsou zadány ve formátu `<principalId>:<roleDefinitionId>`. Více hodnot můžete zadat také pro tuto vlastnost. Pokud je nutné více hodnot, musí být zadán v formuláře `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Více hodnot, jsou oddělené mezerou.
* **identifikátor uri balíčku souboru**: umístění balíčku spravované aplikace, která obsahuje soubory šablon, které může být objekt blob úložiště Azure.

## <a name="next-steps"></a>Další kroky

* Úvod do spravovaných aplikací, najdete v části [spravované aplikace přehled](managed-application-overview.md).
* Příklady souborů najdete v tématu [spravované aplikace ukázky](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).
* Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).
* Informace o publikování spravované aplikace do Azure Marketplace najdete v tématu [Azure spravované aplikace na webu Marketplace](managed-application-author-marketplace.md).
* Informace o využívání spravované aplikace z webu Marketplace najdete v tématu [využívat Azure spravované aplikace na webu Marketplace](managed-application-consume-marketplace.md).
* Naučte se vytvořit soubor definice uživatelského rozhraní pro spravované aplikace, najdete v tématu [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).
