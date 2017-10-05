---
title: "Tajný klíč Key Vault pomocí šablony Resource Manageru | Microsoft Docs"
description: "Ukazuje, jak předat tajného klíče z trezoru klíčů jako parametr během nasazení."
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
ms.openlocfilehash: 1ca72599e67e79d42a3d430dbb13e89ea7265334
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-key-vault-to-pass-secure-parameter-value-during-deployment"></a><span data-ttu-id="79200-103">Předejte hodnotu parametru zabezpečení při nasazení pomocí Key Vault</span><span class="sxs-lookup"><span data-stu-id="79200-103">Use Key Vault to pass secure parameter value during deployment</span></span>

<span data-ttu-id="79200-104">Když potřebujete předat hodnotu zabezpečení (třeba heslo) jako parametr během nasazení, můžete získat hodnotu z [Azure Key Vault](../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="79200-104">When you need to pass a secure value (like a password) as a parameter during deployment, you can retrieve the value from an [Azure Key Vault](../key-vault/key-vault-whatis.md).</span></span> <span data-ttu-id="79200-105">Načíst hodnotu odkazem trezoru klíčů a tajný klíč v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="79200-105">You retrieve the value by referencing the key vault and secret in your parameter file.</span></span> <span data-ttu-id="79200-106">Hodnota je nikdy vystavena, protože budete odkazovat pouze na jeho ID trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="79200-106">The value is never exposed because you only reference its key vault ID.</span></span> <span data-ttu-id="79200-107">Nemusíte ručně zadat hodnotu pro tajný klíč pokaždé, když nasazujete prostředky.</span><span class="sxs-lookup"><span data-stu-id="79200-107">You do not need to manually enter the value for the secret each time you deploy the resources.</span></span> <span data-ttu-id="79200-108">Trezor klíčů může existovat v jiném předplatném. než skupině prostředků, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="79200-108">The key vault can exist in a different subscription than the resource group you are deploying to.</span></span> <span data-ttu-id="79200-109">Při odkazování na trezor klíčů, můžete zahrnout ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="79200-109">When referencing the key vault, you include the subscription ID.</span></span>

<span data-ttu-id="79200-110">Při vytváření trezoru klíčů, nastavte *enabledForTemplateDeployment* vlastnost *true*.</span><span class="sxs-lookup"><span data-stu-id="79200-110">When creating the key vault, set the *enabledForTemplateDeployment* property to *true*.</span></span> <span data-ttu-id="79200-111">Nastavením této hodnoty na hodnotu true, můžete povolit přístup ze šablon Resource Manager během nasazení.</span><span class="sxs-lookup"><span data-stu-id="79200-111">By setting this value to true, you permit access from Resource Manager templates during deployment.</span></span>  

## <a name="deploy-a-key-vault-and-secret"></a><span data-ttu-id="79200-112">Nasazení trezoru klíčů a tajný klíč</span><span class="sxs-lookup"><span data-stu-id="79200-112">Deploy a key vault and secret</span></span>

<span data-ttu-id="79200-113">Vytvoření trezoru klíčů a tajný klíč, pomocí rozhraní příkazového řádku Azure nebo PowerShell.</span><span class="sxs-lookup"><span data-stu-id="79200-113">To create a key vault and secret, use either Azure CLI or PowerShell.</span></span> <span data-ttu-id="79200-114">Všimněte si, že key vault je povolena pro šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="79200-114">Notice that the key vault is enabled for template deployment.</span></span> 

<span data-ttu-id="79200-115">Pokud používáte Azure CLI, použijte:</span><span class="sxs-lookup"><span data-stu-id="79200-115">For Azure CLI, use:</span></span>

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

<span data-ttu-id="79200-116">Pokud používáte PowerShell, použijte:</span><span class="sxs-lookup"><span data-stu-id="79200-116">For PowerShell, use:</span></span>

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a><span data-ttu-id="79200-117">Povolení přístupu k tajný klíč</span><span class="sxs-lookup"><span data-stu-id="79200-117">Enable access to the secret</span></span>

<span data-ttu-id="79200-118">Ať používáte nového trezoru klíčů nebo stávající, zajistí, že uživatel nasazení šablony můžete přístup tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="79200-118">Whether you are using a new key vault or an existing one, ensure that the user deploying the template can access the secret.</span></span> <span data-ttu-id="79200-119">Uživatel nasazení šablonu, která odkazuje na tajný klíč musí mít `Microsoft.KeyVault/vaults/deploy/action` oprávnění pro trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="79200-119">The user deploying a template that references a secret must have the `Microsoft.KeyVault/vaults/deploy/action` permission for the key vault.</span></span> <span data-ttu-id="79200-120">[Vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) a [Přispěvatel](../active-directory/role-based-access-built-in-roles.md#contributor) role obou udělit přístup.</span><span class="sxs-lookup"><span data-stu-id="79200-120">The [Owner](../active-directory/role-based-access-built-in-roles.md#owner) and [Contributor](../active-directory/role-based-access-built-in-roles.md#contributor) roles both grant this access.</span></span> <span data-ttu-id="79200-121">Můžete také vytvořit [vlastní role](../active-directory/role-based-access-control-custom-roles.md) který uděluje toto oprávnění a přidejte uživatele do této role.</span><span class="sxs-lookup"><span data-stu-id="79200-121">You can also create a [custom role](../active-directory/role-based-access-control-custom-roles.md) that grants this permission and add the user to that role.</span></span> <span data-ttu-id="79200-122">Informace o přidání uživatele k roli najdete v tématu [přiřazení uživatele k rolí správce ve službě Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="79200-122">For information about adding a user to a role, see [Assign a user to administrator roles in Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).</span></span>


## <a name="reference-a-secret-with-static-id"></a><span data-ttu-id="79200-123">Referenční tajný klíč s ID statické</span><span class="sxs-lookup"><span data-stu-id="79200-123">Reference a secret with static ID</span></span>

<span data-ttu-id="79200-124">Šablony, která přijímá tajný klíč trezoru klíčů je jako libovolné jiné šablony.</span><span class="sxs-lookup"><span data-stu-id="79200-124">The template that receives a key vault secret is like any other template.</span></span> <span data-ttu-id="79200-125">Je to způsobeno **odkazujete trezoru klíčů v souboru parametrů, není šablona.**</span><span class="sxs-lookup"><span data-stu-id="79200-125">That's because **you reference the key vault in the parameter file, not the template.**</span></span> <span data-ttu-id="79200-126">Například následující šablony nasadí databázi SQL, která zahrnuje heslo správce.</span><span class="sxs-lookup"><span data-stu-id="79200-126">For example, the following template deploys a SQL database that includes an administrator password.</span></span> <span data-ttu-id="79200-127">Parametr hesla je nastaven na zabezpečený řetězec.</span><span class="sxs-lookup"><span data-stu-id="79200-127">The password parameter is set to a secure string.</span></span> <span data-ttu-id="79200-128">Ale šablony neurčuje, kde tato hodnota pochází z.</span><span class="sxs-lookup"><span data-stu-id="79200-128">But, the template does not specify where that value comes from.</span></span>

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

<span data-ttu-id="79200-129">Teď vytvořte soubor parametrů pro předchozí šablonu.</span><span class="sxs-lookup"><span data-stu-id="79200-129">Now, create a parameter file for the preceding template.</span></span> <span data-ttu-id="79200-130">V souboru parametrů zadejte parametr, který odpovídá názvu parametru v šabloně.</span><span class="sxs-lookup"><span data-stu-id="79200-130">In the parameter file, specify a parameter that matches the name of the parameter in the template.</span></span> <span data-ttu-id="79200-131">Pro hodnotu parametru odkazovat tajného klíče z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="79200-131">For the parameter value, reference the secret from the key vault.</span></span> <span data-ttu-id="79200-132">Tajný klíč odkazujete předáním identifikátor prostředku služby key vault a název tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="79200-132">You reference the secret by passing the resource identifier of the key vault and the name of the secret.</span></span> <span data-ttu-id="79200-133">V následujícím příkladu tajný klíč trezoru klíčů již musí existovat a zadejte statickou hodnotu pro jeho ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="79200-133">In the following example, the key vault secret must already exist, and you provide a static value for its resource ID.</span></span>

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

## <a name="reference-a-secret-with-dynamic-id"></a><span data-ttu-id="79200-134">Referenční tajný klíč s dynamické ID</span><span class="sxs-lookup"><span data-stu-id="79200-134">Reference a secret with dynamic ID</span></span>

<span data-ttu-id="79200-135">V předchozí části vám ukázal, jak předat ID statické prostředku pro tajný klíč trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="79200-135">The previous section showed how to pass a static resource ID for the key vault secret.</span></span> <span data-ttu-id="79200-136">Ale v některých scénářích musíte tak, aby odkazovaly trezor klíčů tajný klíč, který se liší podle aktuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="79200-136">However, in some scenarios, you need to reference a key vault secret that varies based on the current deployment.</span></span> <span data-ttu-id="79200-137">V takovém případě nemůžete pevně ID prostředku v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="79200-137">In that case, you cannot hard-code the resource ID in the parameters file.</span></span> <span data-ttu-id="79200-138">Bohužel nelze generovat dynamicky ID prostředku v souboru parametrů vzhledem k tomu, že šablona výrazy nejsou povoleny v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="79200-138">Unfortunately, you cannot dynamically generate the resource ID in the parameters file because template expressions are not permitted in the parameters file.</span></span>

<span data-ttu-id="79200-139">K dynamickému generování ID prostředku pro tajný klíč trezoru klíčů, musíte přesunout prostředek, který potřebuje tajný klíč do vnořené šablony.</span><span class="sxs-lookup"><span data-stu-id="79200-139">To dynamically generate the resource ID for a key vault secret, you must move the resource that needs the secret into a nested template.</span></span> <span data-ttu-id="79200-140">V šabloně hlavní přidání vnořené šablony a předat v parametru, který obsahuje ID dynamicky generovaném prostředku.</span><span class="sxs-lookup"><span data-stu-id="79200-140">In your master template, you add the nested template and pass in a parameter that contains the dynamically generated resource ID.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="79200-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79200-141">Next steps</span></span>
* <span data-ttu-id="79200-142">Obecné informace o trezorů klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="79200-142">For general information about key vaults, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>
* <span data-ttu-id="79200-143">Dokončení příklady odkazující na klíče tajné klíče, naleznete v tématu [Key Vault příklady](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span><span class="sxs-lookup"><span data-stu-id="79200-143">For complete examples of referencing key secrets, see [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span></span>

