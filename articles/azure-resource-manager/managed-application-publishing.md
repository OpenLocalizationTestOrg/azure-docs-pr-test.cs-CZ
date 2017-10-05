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
# <a name="publish-a-managed-application-for-internal-consumption"></a><span data-ttu-id="aa309-103">Publikování spravované aplikace pro interní používání</span><span class="sxs-lookup"><span data-stu-id="aa309-103">Publish a managed application for internal consumption</span></span>

<span data-ttu-id="aa309-104">Můžete vytvářet a publikovat Azure [spravované aplikace](managed-application-overview.md) , jsou určené pro členy vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="aa309-104">You can create and publish Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="aa309-105">IT oddělení například může publikovat spravovaných aplikací, které bylo možné zajistit kompatibilitu s organizační standardy.</span><span class="sxs-lookup"><span data-stu-id="aa309-105">For example, an IT department can publish managed applications that ensure compliance with organizational standards.</span></span> <span data-ttu-id="aa309-106">Tyto spravované aplikace jsou k dispozici prostřednictvím katalogu služeb, není v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="aa309-106">These managed applications are available through the service catalog, not the Azure Marketplace.</span></span>

<span data-ttu-id="aa309-107">Chcete-li publikovat spravované aplikace pro katalogu služeb, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="aa309-107">To publish a managed application for the service catalog, you must:</span></span>

* <span data-ttu-id="aa309-108">Vytvořte balíček ZIP, který obsahuje tři soubory požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="aa309-108">Create a .zip package that contains the three required template files.</span></span>
* <span data-ttu-id="aa309-109">Rozhodněte, které uživatele, skupiny nebo aplikace potřebuje přístup ke skupině prostředků v předplatném uživatele.</span><span class="sxs-lookup"><span data-stu-id="aa309-109">Decide which user, group, or application needs access to the resource group in the user's subscription.</span></span>
* <span data-ttu-id="aa309-110">Vytvořte definici spravované aplikace, která odkazuje na balíček ZIP a požaduje přístup pro identitu.</span><span class="sxs-lookup"><span data-stu-id="aa309-110">Create the managed application definition that points to the .zip package and requests access for the identity.</span></span>

## <a name="create-a-managed-application-package"></a><span data-ttu-id="aa309-111">Vytvořit balíček spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="aa309-111">Create a managed application package</span></span>

<span data-ttu-id="aa309-112">Prvním krokem je vytvoření tři soubory požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="aa309-112">The first step is to create the three required template files.</span></span> <span data-ttu-id="aa309-113">Všechny tři soubory balíčku do souboru ZIP a nahrajte ho do přístupného umístění, jako je například účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="aa309-113">Package all three files into a .zip file, and upload it to an accessible location, such as a storage account.</span></span> <span data-ttu-id="aa309-114">Při vytváření definice spravované aplikace předáte odkaz na tento soubor .zip.</span><span class="sxs-lookup"><span data-stu-id="aa309-114">You pass a link to this .zip file when creating the managed application definition.</span></span>

* <span data-ttu-id="aa309-115">**applianceMainTemplate.json**: Tento soubor definuje prostředky Azure, které jsou zřízené v rámci spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa309-115">**applianceMainTemplate.json**: This file defines the Azure resources that are provisioned as part of the managed application.</span></span> <span data-ttu-id="aa309-116">Šablona je nejsou jiné než regulární šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="aa309-116">The template is no different than a regular Resource Manager template.</span></span> <span data-ttu-id="aa309-117">Například pokud chcete vytvořit účet úložiště prostřednictvím spravované aplikace, applianceMainTemplate.json obsahuje:</span><span class="sxs-lookup"><span data-stu-id="aa309-117">For example, to create a storage account through a managed application, applianceMainTemplate.json contains:</span></span>

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

* <span data-ttu-id="aa309-118">**mainTemplate.json**: uživatelé nasadit tuto šablonu při vytváření spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa309-118">**mainTemplate.json**: Users deploy this template when creating the managed application.</span></span> <span data-ttu-id="aa309-119">Definuje zdroj spravované aplikace, který je typem Microsoft.Solutions/appliances prostředku.</span><span class="sxs-lookup"><span data-stu-id="aa309-119">It defines the managed application resource, which is a Microsoft.Solutions/appliances resource type.</span></span> <span data-ttu-id="aa309-120">Tento soubor obsahuje všechny parametry, které potřebujete k prostředkům v applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="aa309-120">This file contains all the parameters you need for the resources in applianceMainTemplate.json.</span></span>

  <span data-ttu-id="aa309-121">Nastavíte dvě důležité vlastnosti v této šabloně.</span><span class="sxs-lookup"><span data-stu-id="aa309-121">You set two important properties in this template.</span></span> <span data-ttu-id="aa309-122">Nejdřív **applianceDefinitionId** vlastnost je ID definice spravovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="aa309-122">First, the **applianceDefinitionId** property is the ID of the managed application definition.</span></span> <span data-ttu-id="aa309-123">Můžete vytvořit definici později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="aa309-123">You create the definition later in this topic.</span></span> <span data-ttu-id="aa309-124">Při nastavování této hodnoty, musíte rozhodnout, které předplatném nebo skupině prostředků pro ukládání definice spravovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="aa309-124">When setting this value, you must decide which subscription and resource group to use for storing the managed application definitions.</span></span> <span data-ttu-id="aa309-125">A, musíte se rozhodnout na název pro definici.</span><span class="sxs-lookup"><span data-stu-id="aa309-125">And, you must decide on a name for the definition.</span></span> <span data-ttu-id="aa309-126">ID je ve formátu:</span><span class="sxs-lookup"><span data-stu-id="aa309-126">The ID is in the format:</span></span>

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  <span data-ttu-id="aa309-127">Druhý, **managedResourceGroupId** vlastnost je ID skupiny prostředků, kde jsou prostředky Azure vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="aa309-127">Second, the **managedResourceGroupId** property is the ID of the resource group where the Azure resources are created.</span></span> <span data-ttu-id="aa309-128">Můžete přiřadit hodnotu pro tento název skupiny prostředků, nebo mohl uživatel, zadejte název.</span><span class="sxs-lookup"><span data-stu-id="aa309-128">You can assign a value for this resource group name or let the user provide a name.</span></span> <span data-ttu-id="aa309-129">Formát ID je:</span><span class="sxs-lookup"><span data-stu-id="aa309-129">The format of the ID is:</span></span>

  <span data-ttu-id="aa309-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span><span class="sxs-lookup"><span data-stu-id="aa309-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span></span>

  <span data-ttu-id="aa309-131">Následující příklad ukazuje soubor mainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="aa309-131">The following example shows a mainTemplate.json file.</span></span> <span data-ttu-id="aa309-132">Určuje skupinu prostředků pro nasazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="aa309-132">It specifies a resource group for the deployed resources.</span></span> <span data-ttu-id="aa309-133">Definici ID nastaven pro použití s názvem definice **storageApp** ve skupině prostředků s názvem **managedApplicationGroup**.</span><span class="sxs-lookup"><span data-stu-id="aa309-133">The definition ID is set to use a definition named **storageApp** in a resource group named **managedApplicationGroup**.</span></span> <span data-ttu-id="aa309-134">Tyto hodnoty použít jiné názvy, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="aa309-134">You can change these values to use different names.</span></span> <span data-ttu-id="aa309-135">Zadejte vlastní ID předplatného v ID definice.</span><span class="sxs-lookup"><span data-stu-id="aa309-135">Provide your own subscription ID in the definition ID.</span></span>

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

* <span data-ttu-id="aa309-136">**applianceCreateUiDefinition.json**: portál Azure používá tento soubor ke generování uživatelského rozhraní pro uživatele, kteří vytvářejí spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa309-136">**applianceCreateUiDefinition.json**: The Azure portal uses this file to generate the user interface for users who create the managed application.</span></span> <span data-ttu-id="aa309-137">Můžete definovat, jak uživatelé zadali vstup pro jednotlivé parametry.</span><span class="sxs-lookup"><span data-stu-id="aa309-137">You define how users provide input for each parameter.</span></span> <span data-ttu-id="aa309-138">Možnosti můžete použít jako rozevíracího seznamu, textové pole, pole pro heslo a další vstupní nástroje.</span><span class="sxs-lookup"><span data-stu-id="aa309-138">You can use options like a drop-down list, text box, password box, and other input tools.</span></span> <span data-ttu-id="aa309-139">Naučte se vytvořit soubor definice uživatelského rozhraní pro spravované aplikace, najdete v tématu [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aa309-139">To learn how to create a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>

  <span data-ttu-id="aa309-140">Následující příklad ukazuje soubor applianceCreateUiDefinition.json, která umožní uživatelům zadat předponu názvu účtu úložiště prostřednictvím textové pole.</span><span class="sxs-lookup"><span data-stu-id="aa309-140">The following example shows an applianceCreateUiDefinition.json file that enables users to specify the storage account name prefix through a textbox.</span></span>

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

<span data-ttu-id="aa309-141">Po všechny potřebné soubory jsou připravené, jejich zabalení jako soubor ZIP.</span><span class="sxs-lookup"><span data-stu-id="aa309-141">After all the needed files are ready, package them as a .zip file.</span></span> <span data-ttu-id="aa309-142">Tři soubory musí být na kořenové úrovni souboru ZIP.</span><span class="sxs-lookup"><span data-stu-id="aa309-142">The three files must be at the root level of the .zip file.</span></span> <span data-ttu-id="aa309-143">Pokud je vložit do složky, obdržíte chybu při vytváření definice spravovaných aplikací, s oznámením, že nejsou k dispozici požadované soubory.</span><span class="sxs-lookup"><span data-stu-id="aa309-143">If you put them in a folder, you receive an error when creating the managed application definition that states the required files are not present.</span></span> <span data-ttu-id="aa309-144">Nahrání balíčku na dostupné místo z kde ji můžete použít.</span><span class="sxs-lookup"><span data-stu-id="aa309-144">Upload the package to an accessible location from where it can be consumed.</span></span> <span data-ttu-id="aa309-145">Zbývající část tohoto článku předpokládá, že soubor .zip existuje v kontejneru objektů blob veřejně dostupné úložiště.</span><span class="sxs-lookup"><span data-stu-id="aa309-145">The remainder of this article assumes the .zip file exists in a publicly accessible storage blob container.</span></span>

## <a name="create-an-azure-active-directory-user-group-or-application"></a><span data-ttu-id="aa309-146">Vytvoření skupiny uživatelů Azure Active Directory nebo aplikace</span><span class="sxs-lookup"><span data-stu-id="aa309-146">Create an Azure Active Directory user group or application</span></span>

<span data-ttu-id="aa309-147">Druhým krokem je vybrat skupiny uživatelů nebo aplikace pro správu k prostředkům jménem zákazníka.</span><span class="sxs-lookup"><span data-stu-id="aa309-147">The second step is to select a user group or application for managing the resources on behalf of the customer.</span></span> <span data-ttu-id="aa309-148">Této skupiny uživatelů nebo aplikací má oprávnění pro skupinu spravovaných prostředků podle role, která je přiřazena.</span><span class="sxs-lookup"><span data-stu-id="aa309-148">This user group or application has permissions on the managed resource group according to the role that is assigned.</span></span> <span data-ttu-id="aa309-149">Tato role může být žádné předdefinované role řízení přístupu na základě Role (RBAC) jako vlastníka nebo přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="aa309-149">The role can be any built-in Role-Based Access Control (RBAC) role like Owner or Contributor.</span></span> <span data-ttu-id="aa309-150">Také můžete udělit oprávnění jednotlivého uživatele ke správě prostředků, ale obvykle přiřadit toto oprávnění pro skupinu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="aa309-150">You also can give an individual user permission to manage the resources, but typically you assign this permission to a user group.</span></span> <span data-ttu-id="aa309-151">Chcete-li vytvořit novou skupinu uživatelů služby Active Directory, přečtěte si téma [vytvořte skupinu a přidejte členy v Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="aa309-151">To create a new Active Directory user group, see [Create a group and add members in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).</span></span>

<span data-ttu-id="aa309-152">Je třeba ID objektu skupiny uživatelů pro řízení zdrojů.</span><span class="sxs-lookup"><span data-stu-id="aa309-152">You need the object ID of the user group to use for managing the resources.</span></span> <span data-ttu-id="aa309-153">Následující příklad ukazuje, jak získat ID objektu ze skupiny zobrazovaný název:</span><span class="sxs-lookup"><span data-stu-id="aa309-153">The following example shows how to get the object ID from the group's display name:</span></span>

```azurecli-interactive
az ad group show --group exampleGroupName
```

<span data-ttu-id="aa309-154">V ukázkovém příkazu vrátí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="aa309-154">The example command returns the following output:</span></span>

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

<span data-ttu-id="aa309-155">Pro načtení ID objektu, použijte:</span><span class="sxs-lookup"><span data-stu-id="aa309-155">To retrieve just the object ID, use:</span></span>

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-the-role-definition-id"></a><span data-ttu-id="aa309-156">Získání ID definice role</span><span class="sxs-lookup"><span data-stu-id="aa309-156">Get the role definition ID</span></span>

<span data-ttu-id="aa309-157">Dále je nutné zadat ID definice role RBAC předdefinovaná role, které chcete udělit přístup pro uživatele, skupiny uživatelů nebo aplikací.</span><span class="sxs-lookup"><span data-stu-id="aa309-157">Next, you need the role definition ID of the RBAC built-in role you want to grant access to the user, user group, or application.</span></span> <span data-ttu-id="aa309-158">Obvykle použijete roli vlastníka nebo přispěvatele nebo čtečky.</span><span class="sxs-lookup"><span data-stu-id="aa309-158">Typically, you use the Owner or Contributor or Reader role.</span></span> <span data-ttu-id="aa309-159">Následující příkaz ukazuje, jak získat ID definice role pro roli vlastníka:</span><span class="sxs-lookup"><span data-stu-id="aa309-159">The following command shows how to get the role definition ID for the Owner role:</span></span>


```azurecli-interactive
az role definition list --name owner
```

<span data-ttu-id="aa309-160">Tento příkaz vrátí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="aa309-160">That command returns the following output:</span></span>

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

<span data-ttu-id="aa309-161">Hodnota vlastnosti "název" z předchozího příkladu potřebujete.</span><span class="sxs-lookup"><span data-stu-id="aa309-161">You need the value of the "name" property from the preceding example.</span></span> <span data-ttu-id="aa309-162">Můžete získat pouze vlastnosti s:</span><span class="sxs-lookup"><span data-stu-id="aa309-162">You can retrieve just that property with:</span></span>

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-the-managed-application-definition"></a><span data-ttu-id="aa309-163">Vytvořit definici spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="aa309-163">Create the managed application definition</span></span>

<span data-ttu-id="aa309-164">Pokud již jste skupinu prostředků pro ukládání definice spravované aplikace, vytvořte jeden:</span><span class="sxs-lookup"><span data-stu-id="aa309-164">If you do not already have a resource group for storing your managed application definition, create one now:</span></span>

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

<span data-ttu-id="aa309-165">Teď vytvořte prostředek definice spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa309-165">Now, create the managed application definition resource.</span></span>

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

<span data-ttu-id="aa309-166">Parametry použité v předchozím příkladu jsou:</span><span class="sxs-lookup"><span data-stu-id="aa309-166">The parameters used in the preceding example are:</span></span>

* <span data-ttu-id="aa309-167">**Skupina prostředků**: název skupiny prostředků, kde se má vytvořit definici spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa309-167">**resource-group**: The name of the resource group where the managed application definition is created.</span></span>
* <span data-ttu-id="aa309-168">**Úroveň zámku**: typ zámku umístit do skupiny spravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="aa309-168">**lock-level**: The type of lock placed on the managed resource group.</span></span> <span data-ttu-id="aa309-169">Zákazník brání provádění nežádoucího operací v této skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="aa309-169">It prevents the customer from performing undesirable operations on this resource group.</span></span> <span data-ttu-id="aa309-170">V současné době určené jen pro čtení je pouze podporovanou úroveň zámku.</span><span class="sxs-lookup"><span data-stu-id="aa309-170">Currently, ReadOnly is the only supported lock level.</span></span> <span data-ttu-id="aa309-171">Pokud je zadána jen pro čtení, Zákazník může číst pouze ve skupině prostředků spravované prostředky.</span><span class="sxs-lookup"><span data-stu-id="aa309-171">When ReadOnly is specified, the customer can only read the resources present in the managed resource group.</span></span>
* <span data-ttu-id="aa309-172">**povolení**: Popisuje ID objektu zabezpečení a ID definice role, které se používají k udělení oprávnění ke skupině spravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="aa309-172">**authorizations**: Describes the principal ID and the role definition ID that are used to grant permission to the managed resource group.</span></span> <span data-ttu-id="aa309-173">Jsou zadány ve formátu `<principalId>:<roleDefinitionId>`.</span><span class="sxs-lookup"><span data-stu-id="aa309-173">It's specified in the format of `<principalId>:<roleDefinitionId>`.</span></span> <span data-ttu-id="aa309-174">Více hodnot můžete zadat také pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="aa309-174">Multiple values also can be specified for this property.</span></span> <span data-ttu-id="aa309-175">Pokud je nutné více hodnot, musí být zadán v formuláře `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span><span class="sxs-lookup"><span data-stu-id="aa309-175">If multiple values are needed, they should be specified in the form `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span></span> <span data-ttu-id="aa309-176">Více hodnot, jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="aa309-176">Multiple values are separated by a space.</span></span>
* <span data-ttu-id="aa309-177">**identifikátor uri balíčku souboru**: umístění balíčku spravované aplikace, která obsahuje soubory šablon, které může být objekt blob úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="aa309-177">**package-file-uri**: The location of the managed application package that contains the template files, which can be an Azure Storage blob.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa309-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa309-178">Next steps</span></span>

* <span data-ttu-id="aa309-179">Úvod do spravovaných aplikací, najdete v části [spravované aplikace přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aa309-179">For an introduction to managed applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="aa309-180">Příklady souborů najdete v tématu [spravované aplikace ukázky](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="aa309-180">For examples of the files, see [Managed application samples](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span></span>
* <span data-ttu-id="aa309-181">Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="aa309-181">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
* <span data-ttu-id="aa309-182">Informace o publikování spravované aplikace do Azure Marketplace najdete v tématu [Azure spravované aplikace na webu Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="aa309-182">For information about publishing managed applications to the Azure Marketplace, see [Azure managed applications in the Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="aa309-183">Informace o využívání spravované aplikace z webu Marketplace najdete v tématu [využívat Azure spravované aplikace na webu Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="aa309-183">For information about consuming a managed application from the Marketplace, see [Consume Azure managed applications in the Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="aa309-184">Naučte se vytvořit soubor definice uživatelského rozhraní pro spravované aplikace, najdete v tématu [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aa309-184">To learn how to create a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
