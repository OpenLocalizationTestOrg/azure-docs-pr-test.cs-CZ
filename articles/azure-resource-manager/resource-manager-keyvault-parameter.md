---
title: "tajný klíč trezoru aaaKey pomocí šablony Resource Manageru | Microsoft Docs"
description: "Ukazuje, jak toopass tajný klíč z klíče trezoru jako parametr během nasazení."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 0bb7760c95b3b4ef34c9e5cc2e3421be56b5e5e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-key-vault-toopass-secure-parameter-value-during-deployment"></a>Použít hodnotu zabezpečené parametru toopass Key Vault během nasazení

Pokud budete potřebovat toopass zabezpečenou hodnotu (jako jsou hesla) jako parametr během nasazení, můžete načíst hodnotu hello [Azure Key Vault](../key-vault/key-vault-whatis.md). Můžete načíst hodnotu hello odkazem hello trezoru klíčů a tajný klíč v souboru parametrů. Hodnota Hello je nikdy vystavena, protože budete odkazovat pouze na jeho ID trezoru klíčů. Není nutné toomanually zadejte hello hodnotu pro tajný klíč hello pokaždé, když nasazujete hello prostředky. Hello trezoru klíčů může existovat v jiném předplatném. než hello skupinu prostředků, které nasazujete. Při odkazování na hello trezoru klíčů, můžete zahrnout hello ID předplatného.

Při vytváření trezoru klíčů hello nastavte hello *enabledForTemplateDeployment* vlastnost příliš*true*. Nastavením této hodnoty tootrue můžete povolit přístup ze šablon Resource Manager během nasazení.  

## <a name="deploy-a-key-vault-and-secret"></a>Nasazení trezoru klíčů a tajný klíč

toocreate trezoru klíčů a tajný klíč, pomocí rozhraní příkazového řádku Azure nebo PowerShell. Všimněte si, že tento trezor klíčů hello je povolená pro šablonu nasazení. 

Pokud používáte Azure CLI, použijte:

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Pokud používáte PowerShell, použijte:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-toohello-secret"></a>Povolit přístup toohello tajný klíč

Ať používáte nového trezoru klíčů nebo stávající, zajistěte, aby hello nasazení šablony hello mohli uživatelé přistupovat hello tajný klíč. uživatel Hello nasazení šablonu, která odkazuje na tajný klíč musí mít hello `Microsoft.KeyVault/vaults/deploy/action` oprávnění pro trezor klíčů hello. Hello [vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) a [Přispěvatel](../active-directory/role-based-access-built-in-roles.md#contributor) role obou udělit přístup. Můžete také vytvořit [vlastní role](../active-directory/role-based-access-control-custom-roles.md) který uděluje toto oprávnění a přidat roli toothat hello uživatele. Informace o přidávání tooa roli uživatele najdete v tématu [přiřadit uživatele tooadministrator role v Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).


## <a name="reference-a-secret-with-static-id"></a>Referenční tajný klíč s ID statické

Hello šablona, která přijímá tajný klíč trezoru klíčů je jako libovolné jiné šablony. Je to způsobeno **odkazujete hello trezoru klíčů v souboru parametrů hello, není hello šablony.** Například následující šablony hello nasadí databázi SQL, která zahrnuje heslo správce. Parametr hesla Hello nastavena tooa zabezpečený řetězec. Ale hello šablony neurčuje, kde tato hodnota pochází z.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "type": "string"
        },
        "administratorLoginPassword": {
            "type": "securestring"
        },
        "collation": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        },
        "edition": {
            "type": "string"
        },
        "requestedServiceObjectiveId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "maxSizeBytes": {
            "type": "string"
        },
        "serverName": {
            "type": "string"
        },
        "sampleName": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "properties": {
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "version": "12.0"
            },
            "resources": [
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "[parameters('databaseName')]",
                    "properties": {
                        "collation": "[parameters('collation')]",
                        "edition": "[parameters('edition')]",
                        "maxSizeBytes": "[parameters('maxSizeBytes')]",
                        "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]",
                        "sampleName": "[parameters('sampleName')]"
                    },
                    "type": "databases"
                },
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "AllowAllWindowsAzureIps",
                    "properties": {
                        "endIpAddress": "0.0.0.0",
                        "startIpAddress": "0.0.0.0"
                    },
                    "type": "firewallrules"
                }
            ],
            "type": "Microsoft.Sql/servers"
        }
    ]
}
```

Teď vytvořte soubor parametrů pro hello předcházející šablony. V souboru parametrů hello zadejte parametr, který odpovídá názvu hello hello parametru v šabloně hello. Hodnota parametru hello referenční hello tajných klíčů v trezoru hello. Tajný klíč hello odkazujete předáním hello identifikátor prostředku trezoru klíčů hello a hello název hello tajný klíč. V následujícím příkladu hello tajný klíč trezoru klíčů hello již musí existovat a zadejte statickou hodnotu pro jeho ID prostředku.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "value": "exampleadmin"
        },
        "administratorLoginPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/{guid}/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/{vault-name}"
              },
              "secretName": "examplesecret"
            }
        },
        "collation": {
            "value": "SQL_Latin1_General_CP1_CI_AS"
        },
        "databaseName": {
            "value": "exampledb"
        },
        "edition": {
            "value": "Standard"
        },
        "location": {
            "value": "southcentralus"
        },
        "maxSizeBytes": {
            "value": "268435456000"
        },
        "requestedServiceObjectiveId": {
            "value": "455330e1-00cd-488b-b5fa-177c226f28b7"
        },
        "sampleName": {
            "value": ""
        },
        "serverName": {
            "value": "exampleserver"
        }
    }
}
```

## <a name="reference-a-secret-with-dynamic-id"></a>Referenční tajný klíč s dynamické ID

předchozí části Hello vám ukázal, jak toopass ID statické prostředku pro hello klíč trezoru tajný klíč. Ale v některých scénářích musíte tooreference trezoru klíčů tajný klíč, který se liší podle hello aktuální nasazení. V takovém případě nemůžete ID prostředku hello pevný kód v souboru parametrů hello. Bohužel nelze generovat dynamicky hello ID prostředku v souboru parametrů hello vzhledem k tomu, že šablona výrazy nejsou povoleny v souboru parametrů hello.

toodynamically generovat hello ID prostředku pro tajný klíč trezoru klíčů, je nutné přesunout hello prostředek, který potřebuje hello tajný klíč do vnořené šablony. V šabloně hlavní přidání hello vnořené šablony a předat v parametru, který obsahuje ID hello dynamicky generované prostředku.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a>Další kroky
* Obecné informace o trezorů klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).
* Dokončení příklady odkazující na klíče tajné klíče, naleznete v tématu [Key Vault příklady](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

