---
title: "aaaCreate a publikování aplikace spravované katalogu služby Azure | Microsoft Docs"
description: "Ukazuje, jak toocreate Azure spravované aplikace, která je určena pro členy vaší organizace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 31f2f9e3b50f57dae7f4dcf2edefa7366bfff25c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Publikování spravované aplikace pro interní používání

Můžete vytvářet a publikovat Azure [spravované aplikace](managed-application-overview.md) , jsou určené pro členy vaší organizace. IT oddělení například může publikovat spravovaných aplikací, které bylo možné zajistit kompatibilitu s organizační standardy. Tyto spravované aplikace jsou k dispozici prostřednictvím hello katalogu služeb, hello Azure Marketplace.

toopublish spravované aplikace hello katalogu služeb, které je nutné:

* Vytvořte balíček ZIP, který obsahuje tři soubory požadovanou šablonu hello.
* Rozhodněte, které uživatele, skupiny nebo aplikace potřebuje přístup k toohello skupiny prostředků v předplatném hello uživatele.
* Vytvořte definici hello spravované aplikace, který odkazuje balíček ZIP toohello a požaduje přístup pro identitu hello.

## <a name="create-a-managed-application-package"></a>Vytvořit balíček spravované aplikace

prvním krokem Hello je toocreate hello tři soubory požadované šablony. Všechny tři soubory balíčku do souboru ZIP a nahrajte ho tooan přístupného umístění, jako je například účet úložiště. Soubor ZIP toothis odkaz předáte při vytváření hello spravovaných definice aplikace.

* **applianceMainTemplate.json**: Tento soubor definuje hello Azure prostředky, které jsou zřízené v rámci hello spravované aplikace. Šablona Hello je nejsou jiné než regulární šablony Resource Manageru. Například toocreate účet úložiště prostřednictvím spravované aplikace, applianceMainTemplate.json obsahuje:

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

* **mainTemplate.json**: uživatelé nasadit tuto šablonu při vytváření hello spravované aplikace. Definuje hello spravované aplikace zdroj, který je typem Microsoft.Solutions/appliances prostředku. Tento soubor obsahuje všechny hello parametry, které potřebujete k hello prostředky v applianceMainTemplate.json.

  Nastavíte dvě důležité vlastnosti v této šabloně. Nejprve hello **applianceDefinitionId** vlastnost je hello ID definice hello spravované aplikace. Můžete vytvořit definici hello později v tomto tématu. Při nastavování této hodnoty, musíte se rozhodnout, jaké předplatné a toouse skupiny prostředků pro ukládání hello Definice spravovaných aplikací. A, musíte se rozhodnout na název pro definici hello. Hello ID je ve formátu hello:

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  Za druhé hello **managedResourceGroupId** vlastnost je ID hello hello skupiny prostředků, kde hello prostředky Azure se vytvářejí. Můžete přiřadit hodnotu pro tento název skupiny prostředků, nebo mohl uživatel hello zadejte název. Formát Hello hello ID je:

  `/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.

  Hello následující příklad ukazuje soubor mainTemplate.json. Určuje skupinu prostředků pro hello nasazené prostředky. Hello ID definice je toouse sadu s názvem definice **storageApp** ve skupině prostředků s názvem **managedApplicationGroup**. Tyto hodnoty odlišné názvy toouse, můžete změnit. Zadejte vlastní ID předplatného v ID hello definice.

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

* **applianceCreateUiDefinition.json**: hello portál Azure používá tento soubor toogenerate hello uživatelské rozhraní pro uživatele, kteří vytvářejí hello spravované aplikace. Můžete definovat, jak uživatelé zadali vstup pro jednotlivé parametry. Možnosti můžete použít jako rozevíracího seznamu, textové pole, pole pro heslo a další vstupní nástroje. jak toocreate soubor definice uživatelského rozhraní pro spravované aplikace, najdete v části toolearn [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).

  Hello následující příklad ukazuje soubor applianceCreateUiDefinition.json, který umožňuje uživatelům toospecify hello úložiště účet název předpony prostřednictvím textové pole.

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
                "toolTip": "Provide a value that is used for hello prefix of your storage account. Limit too11 characters.",
                "constraints": {
                    "required": true,
                    "regex": "^[a-z0-9A-Z]{1,11}$",
                    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-11 characters long."
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

Po všechny hello potřebné soubory jsou připravené, jejich zabalení jako soubor ZIP. Hello tři soubory musí být na kořenové úrovni hello hello soubor .zip. Pokud umístí je do složky, obdržíte chybu při vytváření hello spravovaných definice aplikace, která s oznámením, že hello požadované soubory neexistují. Nahrajte hello balíček tooan přístupné umístění, ze kterých může zpracovat. Hello zbývající část tohoto článku předpokládá, že soubor .zip hello existuje v kontejneru objektů blob veřejně dostupné úložiště.

## <a name="create-an-azure-active-directory-user-group-or-application"></a>Vytvoření skupiny uživatelů Azure Active Directory nebo aplikace

druhý krok Hello je tooselect skupinu uživatelů nebo aplikace pro správu prostředků hello jménem zákazníka hello. Této skupiny uživatelů nebo aplikací má oprávnění pro hello spravovaných prostředků skupiny podle toohello role, která je přiřazena. Hello role může být žádné předdefinované role řízení přístupu na základě Role (RBAC) jako vlastníka nebo přispěvatele. Také můžete udělit jednotlivých uživatelských oprávnění toomanage hello prostředků, ale obvykle přiřadit této skupiny uživatelů tooa oprávnění. toocreate novou skupinu uživatelů služby Active Directory, najdete v části [vytvořte skupinu a přidejte členy v Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

Potřebujete ID objektu hello hello uživatelské skupiny toouse pro správu prostředků hello. Hello následující příklad ukazuje, jak tooget hello ID objektu ze skupiny hello zobrazovaný název:

```azurecli-interactive
az ad group show --group exampleGroupName
```

Hello Ukázkový příkaz vrátí hello následující výstup:

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

tooretrieve jenom hello ID objektu, použijte:

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-hello-role-definition-id"></a>Získání ID definice role hello

Dále je nutné zadat ID definice role hello hello předdefinovaná role RBAC chcete toogrant přístup toohello uživatele, skupiny uživatelů nebo aplikací. Obvykle použijete roli Přispěvatel nebo Čtenář nebo hello vlastníka. Hello následující příkaz ukazuje, jak tooget hello ID definice role pro roli vlastníka hello:


```azurecli-interactive
az role definition list --name owner
```

Tento příkaz vrátí hello následující výstup:

```azurecli
{
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
    "properties": {
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything, including access tooresources.",
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

Je nutné hello hodnota vlastnosti "název" hello z předchozích příklad hello. Můžete získat pouze vlastnosti s:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-hello-managed-application-definition"></a>Vytvořit definici hello spravované aplikace

Pokud již jste skupinu prostředků pro ukládání definice spravované aplikace, vytvořte jeden:

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

Teď vytvořte hello spravované aplikace definice prostředků.

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

jsou použité v předchozím příkladu hello Hello parametry:

* **Skupina prostředků**: hello název skupiny prostředků hello které hello spravovaná definice aplikace se vytvoří.
* **Úroveň zámku**: hello typ zámku umístit do skupiny hello spravovaných prostředků. Zákazník hello brání provádění nežádoucího operací v této skupině prostředků. V současné době určené jen pro čtení je hello podporuje jenom úroveň zámku. Pokud je zadána jen pro čtení, hello zákazníka můžete číst jenom hello prostředky ve skupině hello spravovaných prostředků.
* **povolení**: Popisuje hello ID objektu zabezpečení a hello ID definice role, které jsou používané toogrant oprávnění toohello spravovaných prostředků skupiny. Jsou zadány ve formátu hello `<principalId>:<roleDefinitionId>`. Více hodnot můžete zadat také pro tuto vlastnost. Pokud je nutné více hodnot, musí být zadán v hello formuláře `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Více hodnot, jsou oddělené mezerou.
* **identifikátor uri balíčku souboru**: hello umístění balíčku hello spravované aplikace, který obsahuje soubory hello šablony, které může být objekt blob úložiště Azure.

## <a name="next-steps"></a>Další kroky

* Úvod toomanaged aplikace naleznete v [spravované aplikace přehled](managed-application-overview.md).
* Příklady hello souborů najdete v tématu [spravované aplikace ukázky](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).
* Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).
* Informace o publikování toohello spravovaných aplikací Azure Marketplace najdete v tématu [Azure spravované aplikace v hello Marketplace](managed-application-author-marketplace.md).
* Informace o použití spravovaných aplikací z hello Marketplace najdete v tématu [využívat Azure spravované aplikace v hello Marketplace](managed-application-consume-marketplace.md).
* jak toocreate soubor definice uživatelského rozhraní pro spravované aplikace, najdete v části toolearn [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).
