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
# <a name="use-key-vault-toopass-secure-parameter-value-during-deployment"></a><span data-ttu-id="84e1d-103">Použít hodnotu zabezpečené parametru toopass Key Vault během nasazení</span><span class="sxs-lookup"><span data-stu-id="84e1d-103">Use Key Vault toopass secure parameter value during deployment</span></span>

<span data-ttu-id="84e1d-104">Pokud budete potřebovat toopass zabezpečenou hodnotu (jako jsou hesla) jako parametr během nasazení, můžete načíst hodnotu hello [Azure Key Vault](../key-vault/key-vault-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84e1d-104">When you need toopass a secure value (like a password) as a parameter during deployment, you can retrieve hello value from an [Azure Key Vault](../key-vault/key-vault-whatis.md).</span></span> <span data-ttu-id="84e1d-105">Můžete načíst hodnotu hello odkazem hello trezoru klíčů a tajný klíč v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="84e1d-105">You retrieve hello value by referencing hello key vault and secret in your parameter file.</span></span> <span data-ttu-id="84e1d-106">Hodnota Hello je nikdy vystavena, protože budete odkazovat pouze na jeho ID trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="84e1d-106">hello value is never exposed because you only reference its key vault ID.</span></span> <span data-ttu-id="84e1d-107">Není nutné toomanually zadejte hello hodnotu pro tajný klíč hello pokaždé, když nasazujete hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="84e1d-107">You do not need toomanually enter hello value for hello secret each time you deploy hello resources.</span></span> <span data-ttu-id="84e1d-108">Hello trezoru klíčů může existovat v jiném předplatném. než hello skupinu prostředků, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="84e1d-108">hello key vault can exist in a different subscription than hello resource group you are deploying to.</span></span> <span data-ttu-id="84e1d-109">Při odkazování na hello trezoru klíčů, můžete zahrnout hello ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="84e1d-109">When referencing hello key vault, you include hello subscription ID.</span></span>

<span data-ttu-id="84e1d-110">Při vytváření trezoru klíčů hello nastavte hello *enabledForTemplateDeployment* vlastnost příliš*true*.</span><span class="sxs-lookup"><span data-stu-id="84e1d-110">When creating hello key vault, set hello *enabledForTemplateDeployment* property too*true*.</span></span> <span data-ttu-id="84e1d-111">Nastavením této hodnoty tootrue můžete povolit přístup ze šablon Resource Manager během nasazení.</span><span class="sxs-lookup"><span data-stu-id="84e1d-111">By setting this value tootrue, you permit access from Resource Manager templates during deployment.</span></span>  

## <a name="deploy-a-key-vault-and-secret"></a><span data-ttu-id="84e1d-112">Nasazení trezoru klíčů a tajný klíč</span><span class="sxs-lookup"><span data-stu-id="84e1d-112">Deploy a key vault and secret</span></span>

<span data-ttu-id="84e1d-113">toocreate trezoru klíčů a tajný klíč, pomocí rozhraní příkazového řádku Azure nebo PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84e1d-113">toocreate a key vault and secret, use either Azure CLI or PowerShell.</span></span> <span data-ttu-id="84e1d-114">Všimněte si, že tento trezor klíčů hello je povolená pro šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="84e1d-114">Notice that hello key vault is enabled for template deployment.</span></span> 

<span data-ttu-id="84e1d-115">Pokud používáte Azure CLI, použijte:</span><span class="sxs-lookup"><span data-stu-id="84e1d-115">For Azure CLI, use:</span></span>

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

<span data-ttu-id="84e1d-116">Pokud používáte PowerShell, použijte:</span><span class="sxs-lookup"><span data-stu-id="84e1d-116">For PowerShell, use:</span></span>

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-toohello-secret"></a><span data-ttu-id="84e1d-117">Povolit přístup toohello tajný klíč</span><span class="sxs-lookup"><span data-stu-id="84e1d-117">Enable access toohello secret</span></span>

<span data-ttu-id="84e1d-118">Ať používáte nového trezoru klíčů nebo stávající, zajistěte, aby hello nasazení šablony hello mohli uživatelé přistupovat hello tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="84e1d-118">Whether you are using a new key vault or an existing one, ensure that hello user deploying hello template can access hello secret.</span></span> <span data-ttu-id="84e1d-119">uživatel Hello nasazení šablonu, která odkazuje na tajný klíč musí mít hello `Microsoft.KeyVault/vaults/deploy/action` oprávnění pro trezor klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="84e1d-119">hello user deploying a template that references a secret must have hello `Microsoft.KeyVault/vaults/deploy/action` permission for hello key vault.</span></span> <span data-ttu-id="84e1d-120">Hello [vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) a [Přispěvatel](../active-directory/role-based-access-built-in-roles.md#contributor) role obou udělit přístup.</span><span class="sxs-lookup"><span data-stu-id="84e1d-120">hello [Owner](../active-directory/role-based-access-built-in-roles.md#owner) and [Contributor](../active-directory/role-based-access-built-in-roles.md#contributor) roles both grant this access.</span></span> <span data-ttu-id="84e1d-121">Můžete také vytvořit [vlastní role](../active-directory/role-based-access-control-custom-roles.md) který uděluje toto oprávnění a přidat roli toothat hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="84e1d-121">You can also create a [custom role](../active-directory/role-based-access-control-custom-roles.md) that grants this permission and add hello user toothat role.</span></span> <span data-ttu-id="84e1d-122">Informace o přidávání tooa roli uživatele najdete v tématu [přiřadit uživatele tooadministrator role v Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="84e1d-122">For information about adding a user tooa role, see [Assign a user tooadministrator roles in Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).</span></span>


## <a name="reference-a-secret-with-static-id"></a><span data-ttu-id="84e1d-123">Referenční tajný klíč s ID statické</span><span class="sxs-lookup"><span data-stu-id="84e1d-123">Reference a secret with static ID</span></span>

<span data-ttu-id="84e1d-124">Hello šablona, která přijímá tajný klíč trezoru klíčů je jako libovolné jiné šablony.</span><span class="sxs-lookup"><span data-stu-id="84e1d-124">hello template that receives a key vault secret is like any other template.</span></span> <span data-ttu-id="84e1d-125">Je to způsobeno **odkazujete hello trezoru klíčů v souboru parametrů hello, není hello šablony.**</span><span class="sxs-lookup"><span data-stu-id="84e1d-125">That's because **you reference hello key vault in hello parameter file, not hello template.**</span></span> <span data-ttu-id="84e1d-126">Například následující šablony hello nasadí databázi SQL, která zahrnuje heslo správce.</span><span class="sxs-lookup"><span data-stu-id="84e1d-126">For example, hello following template deploys a SQL database that includes an administrator password.</span></span> <span data-ttu-id="84e1d-127">Parametr hesla Hello nastavena tooa zabezpečený řetězec.</span><span class="sxs-lookup"><span data-stu-id="84e1d-127">hello password parameter is set tooa secure string.</span></span> <span data-ttu-id="84e1d-128">Ale hello šablony neurčuje, kde tato hodnota pochází z.</span><span class="sxs-lookup"><span data-stu-id="84e1d-128">But, hello template does not specify where that value comes from.</span></span>

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

<span data-ttu-id="84e1d-129">Teď vytvořte soubor parametrů pro hello předcházející šablony.</span><span class="sxs-lookup"><span data-stu-id="84e1d-129">Now, create a parameter file for hello preceding template.</span></span> <span data-ttu-id="84e1d-130">V souboru parametrů hello zadejte parametr, který odpovídá názvu hello hello parametru v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="84e1d-130">In hello parameter file, specify a parameter that matches hello name of hello parameter in hello template.</span></span> <span data-ttu-id="84e1d-131">Hodnota parametru hello referenční hello tajných klíčů v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="84e1d-131">For hello parameter value, reference hello secret from hello key vault.</span></span> <span data-ttu-id="84e1d-132">Tajný klíč hello odkazujete předáním hello identifikátor prostředku trezoru klíčů hello a hello název hello tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="84e1d-132">You reference hello secret by passing hello resource identifier of hello key vault and hello name of hello secret.</span></span> <span data-ttu-id="84e1d-133">V následujícím příkladu hello tajný klíč trezoru klíčů hello již musí existovat a zadejte statickou hodnotu pro jeho ID prostředku.</span><span class="sxs-lookup"><span data-stu-id="84e1d-133">In hello following example, hello key vault secret must already exist, and you provide a static value for its resource ID.</span></span>

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

## <a name="reference-a-secret-with-dynamic-id"></a><span data-ttu-id="84e1d-134">Referenční tajný klíč s dynamické ID</span><span class="sxs-lookup"><span data-stu-id="84e1d-134">Reference a secret with dynamic ID</span></span>

<span data-ttu-id="84e1d-135">předchozí části Hello vám ukázal, jak toopass ID statické prostředku pro hello klíč trezoru tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="84e1d-135">hello previous section showed how toopass a static resource ID for hello key vault secret.</span></span> <span data-ttu-id="84e1d-136">Ale v některých scénářích musíte tooreference trezoru klíčů tajný klíč, který se liší podle hello aktuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="84e1d-136">However, in some scenarios, you need tooreference a key vault secret that varies based on hello current deployment.</span></span> <span data-ttu-id="84e1d-137">V takovém případě nemůžete ID prostředku hello pevný kód v souboru parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="84e1d-137">In that case, you cannot hard-code hello resource ID in hello parameters file.</span></span> <span data-ttu-id="84e1d-138">Bohužel nelze generovat dynamicky hello ID prostředku v souboru parametrů hello vzhledem k tomu, že šablona výrazy nejsou povoleny v souboru parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="84e1d-138">Unfortunately, you cannot dynamically generate hello resource ID in hello parameters file because template expressions are not permitted in hello parameters file.</span></span>

<span data-ttu-id="84e1d-139">toodynamically generovat hello ID prostředku pro tajný klíč trezoru klíčů, je nutné přesunout hello prostředek, který potřebuje hello tajný klíč do vnořené šablony.</span><span class="sxs-lookup"><span data-stu-id="84e1d-139">toodynamically generate hello resource ID for a key vault secret, you must move hello resource that needs hello secret into a nested template.</span></span> <span data-ttu-id="84e1d-140">V šabloně hlavní přidání hello vnořené šablony a předat v parametru, který obsahuje ID hello dynamicky generované prostředku.</span><span class="sxs-lookup"><span data-stu-id="84e1d-140">In your master template, you add hello nested template and pass in a parameter that contains hello dynamically generated resource ID.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="84e1d-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84e1d-141">Next steps</span></span>
* <span data-ttu-id="84e1d-142">Obecné informace o trezorů klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="84e1d-142">For general information about key vaults, see [Get started with Azure Key Vault](../key-vault/key-vault-get-started.md).</span></span>
* <span data-ttu-id="84e1d-143">Dokončení příklady odkazující na klíče tajné klíče, naleznete v tématu [Key Vault příklady](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span><span class="sxs-lookup"><span data-stu-id="84e1d-143">For complete examples of referencing key secrets, see [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).</span></span>

