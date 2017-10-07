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
# <a name="publish-a-managed-application-for-internal-consumption"></a><span data-ttu-id="7ab5f-103">Publikování spravované aplikace pro interní používání</span><span class="sxs-lookup"><span data-stu-id="7ab5f-103">Publish a managed application for internal consumption</span></span>

<span data-ttu-id="7ab5f-104">Můžete vytvářet a publikovat Azure [spravované aplikace](managed-application-overview.md) , jsou určené pro členy vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-104">You can create and publish Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="7ab5f-105">IT oddělení například může publikovat spravovaných aplikací, které bylo možné zajistit kompatibilitu s organizační standardy.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-105">For example, an IT department can publish managed applications that ensure compliance with organizational standards.</span></span> <span data-ttu-id="7ab5f-106">Tyto spravované aplikace jsou k dispozici prostřednictvím hello katalogu služeb, hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-106">These managed applications are available through hello service catalog, not hello Azure Marketplace.</span></span>

<span data-ttu-id="7ab5f-107">toopublish spravované aplikace hello katalogu služeb, které je nutné:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-107">toopublish a managed application for hello service catalog, you must:</span></span>

* <span data-ttu-id="7ab5f-108">Vytvořte balíček ZIP, který obsahuje tři soubory požadovanou šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-108">Create a .zip package that contains hello three required template files.</span></span>
* <span data-ttu-id="7ab5f-109">Rozhodněte, které uživatele, skupiny nebo aplikace potřebuje přístup k toohello skupiny prostředků v předplatném hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-109">Decide which user, group, or application needs access toohello resource group in hello user's subscription.</span></span>
* <span data-ttu-id="7ab5f-110">Vytvořte definici hello spravované aplikace, který odkazuje balíček ZIP toohello a požaduje přístup pro identitu hello.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-110">Create hello managed application definition that points toohello .zip package and requests access for hello identity.</span></span>

## <a name="create-a-managed-application-package"></a><span data-ttu-id="7ab5f-111">Vytvořit balíček spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="7ab5f-111">Create a managed application package</span></span>

<span data-ttu-id="7ab5f-112">prvním krokem Hello je toocreate hello tři soubory požadované šablony.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-112">hello first step is toocreate hello three required template files.</span></span> <span data-ttu-id="7ab5f-113">Všechny tři soubory balíčku do souboru ZIP a nahrajte ho tooan přístupného umístění, jako je například účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-113">Package all three files into a .zip file, and upload it tooan accessible location, such as a storage account.</span></span> <span data-ttu-id="7ab5f-114">Soubor ZIP toothis odkaz předáte při vytváření hello spravovaných definice aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-114">You pass a link toothis .zip file when creating hello managed application definition.</span></span>

* <span data-ttu-id="7ab5f-115">**applianceMainTemplate.json**: Tento soubor definuje hello Azure prostředky, které jsou zřízené v rámci hello spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-115">**applianceMainTemplate.json**: This file defines hello Azure resources that are provisioned as part of hello managed application.</span></span> <span data-ttu-id="7ab5f-116">Šablona Hello je nejsou jiné než regulární šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-116">hello template is no different than a regular Resource Manager template.</span></span> <span data-ttu-id="7ab5f-117">Například toocreate účet úložiště prostřednictvím spravované aplikace, applianceMainTemplate.json obsahuje:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-117">For example, toocreate a storage account through a managed application, applianceMainTemplate.json contains:</span></span>

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

* <span data-ttu-id="7ab5f-118">**mainTemplate.json**: uživatelé nasadit tuto šablonu při vytváření hello spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-118">**mainTemplate.json**: Users deploy this template when creating hello managed application.</span></span> <span data-ttu-id="7ab5f-119">Definuje hello spravované aplikace zdroj, který je typem Microsoft.Solutions/appliances prostředku.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-119">It defines hello managed application resource, which is a Microsoft.Solutions/appliances resource type.</span></span> <span data-ttu-id="7ab5f-120">Tento soubor obsahuje všechny hello parametry, které potřebujete k hello prostředky v applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-120">This file contains all hello parameters you need for hello resources in applianceMainTemplate.json.</span></span>

  <span data-ttu-id="7ab5f-121">Nastavíte dvě důležité vlastnosti v této šabloně.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-121">You set two important properties in this template.</span></span> <span data-ttu-id="7ab5f-122">Nejprve hello **applianceDefinitionId** vlastnost je hello ID definice hello spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-122">First, hello **applianceDefinitionId** property is hello ID of hello managed application definition.</span></span> <span data-ttu-id="7ab5f-123">Můžete vytvořit definici hello později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-123">You create hello definition later in this topic.</span></span> <span data-ttu-id="7ab5f-124">Při nastavování této hodnoty, musíte se rozhodnout, jaké předplatné a toouse skupiny prostředků pro ukládání hello Definice spravovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-124">When setting this value, you must decide which subscription and resource group toouse for storing hello managed application definitions.</span></span> <span data-ttu-id="7ab5f-125">A, musíte se rozhodnout na název pro definici hello.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-125">And, you must decide on a name for hello definition.</span></span> <span data-ttu-id="7ab5f-126">Hello ID je ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-126">hello ID is in hello format:</span></span>

  `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Solutions/applianceDefinitions/<definition-name>`

  <span data-ttu-id="7ab5f-127">Za druhé hello **managedResourceGroupId** vlastnost je ID hello hello skupiny prostředků, kde hello prostředky Azure se vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-127">Second, hello **managedResourceGroupId** property is hello ID of hello resource group where hello Azure resources are created.</span></span> <span data-ttu-id="7ab5f-128">Můžete přiřadit hodnotu pro tento název skupiny prostředků, nebo mohl uživatel hello zadejte název.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-128">You can assign a value for this resource group name or let hello user provide a name.</span></span> <span data-ttu-id="7ab5f-129">Formát Hello hello ID je:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-129">hello format of hello ID is:</span></span>

  <span data-ttu-id="7ab5f-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-130">`/subscriptions/<subscription-id>/resourceGroups/<resoure-group-name>`.</span></span>

  <span data-ttu-id="7ab5f-131">Hello následující příklad ukazuje soubor mainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-131">hello following example shows a mainTemplate.json file.</span></span> <span data-ttu-id="7ab5f-132">Určuje skupinu prostředků pro hello nasazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-132">It specifies a resource group for hello deployed resources.</span></span> <span data-ttu-id="7ab5f-133">Hello ID definice je toouse sadu s názvem definice **storageApp** ve skupině prostředků s názvem **managedApplicationGroup**.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-133">hello definition ID is set toouse a definition named **storageApp** in a resource group named **managedApplicationGroup**.</span></span> <span data-ttu-id="7ab5f-134">Tyto hodnoty odlišné názvy toouse, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-134">You can change these values toouse different names.</span></span> <span data-ttu-id="7ab5f-135">Zadejte vlastní ID předplatného v ID hello definice.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-135">Provide your own subscription ID in hello definition ID.</span></span>

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

* <span data-ttu-id="7ab5f-136">**applianceCreateUiDefinition.json**: hello portál Azure používá tento soubor toogenerate hello uživatelské rozhraní pro uživatele, kteří vytvářejí hello spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-136">**applianceCreateUiDefinition.json**: hello Azure portal uses this file toogenerate hello user interface for users who create hello managed application.</span></span> <span data-ttu-id="7ab5f-137">Můžete definovat, jak uživatelé zadali vstup pro jednotlivé parametry.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-137">You define how users provide input for each parameter.</span></span> <span data-ttu-id="7ab5f-138">Možnosti můžete použít jako rozevíracího seznamu, textové pole, pole pro heslo a další vstupní nástroje.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-138">You can use options like a drop-down list, text box, password box, and other input tools.</span></span> <span data-ttu-id="7ab5f-139">jak toocreate soubor definice uživatelského rozhraní pro spravované aplikace, najdete v části toolearn [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7ab5f-139">toolearn how toocreate a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>

  <span data-ttu-id="7ab5f-140">Hello následující příklad ukazuje soubor applianceCreateUiDefinition.json, který umožňuje uživatelům toospecify hello úložiště účet název předpony prostřednictvím textové pole.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-140">hello following example shows an applianceCreateUiDefinition.json file that enables users toospecify hello storage account name prefix through a textbox.</span></span>

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

<span data-ttu-id="7ab5f-141">Po všechny hello potřebné soubory jsou připravené, jejich zabalení jako soubor ZIP.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-141">After all hello needed files are ready, package them as a .zip file.</span></span> <span data-ttu-id="7ab5f-142">Hello tři soubory musí být na kořenové úrovni hello hello soubor .zip.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-142">hello three files must be at hello root level of hello .zip file.</span></span> <span data-ttu-id="7ab5f-143">Pokud umístí je do složky, obdržíte chybu při vytváření hello spravovaných definice aplikace, která s oznámením, že hello požadované soubory neexistují.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-143">If you put them in a folder, you receive an error when creating hello managed application definition that states hello required files are not present.</span></span> <span data-ttu-id="7ab5f-144">Nahrajte hello balíček tooan přístupné umístění, ze kterých může zpracovat.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-144">Upload hello package tooan accessible location from where it can be consumed.</span></span> <span data-ttu-id="7ab5f-145">Hello zbývající část tohoto článku předpokládá, že soubor .zip hello existuje v kontejneru objektů blob veřejně dostupné úložiště.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-145">hello remainder of this article assumes hello .zip file exists in a publicly accessible storage blob container.</span></span>

## <a name="create-an-azure-active-directory-user-group-or-application"></a><span data-ttu-id="7ab5f-146">Vytvoření skupiny uživatelů Azure Active Directory nebo aplikace</span><span class="sxs-lookup"><span data-stu-id="7ab5f-146">Create an Azure Active Directory user group or application</span></span>

<span data-ttu-id="7ab5f-147">druhý krok Hello je tooselect skupinu uživatelů nebo aplikace pro správu prostředků hello jménem zákazníka hello.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-147">hello second step is tooselect a user group or application for managing hello resources on behalf of hello customer.</span></span> <span data-ttu-id="7ab5f-148">Této skupiny uživatelů nebo aplikací má oprávnění pro hello spravovaných prostředků skupiny podle toohello role, která je přiřazena.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-148">This user group or application has permissions on hello managed resource group according toohello role that is assigned.</span></span> <span data-ttu-id="7ab5f-149">Hello role může být žádné předdefinované role řízení přístupu na základě Role (RBAC) jako vlastníka nebo přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-149">hello role can be any built-in Role-Based Access Control (RBAC) role like Owner or Contributor.</span></span> <span data-ttu-id="7ab5f-150">Také můžete udělit jednotlivých uživatelských oprávnění toomanage hello prostředků, ale obvykle přiřadit této skupiny uživatelů tooa oprávnění.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-150">You also can give an individual user permission toomanage hello resources, but typically you assign this permission tooa user group.</span></span> <span data-ttu-id="7ab5f-151">toocreate novou skupinu uživatelů služby Active Directory, najdete v části [vytvořte skupinu a přidejte členy v Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7ab5f-151">toocreate a new Active Directory user group, see [Create a group and add members in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).</span></span>

<span data-ttu-id="7ab5f-152">Potřebujete ID objektu hello hello uživatelské skupiny toouse pro správu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-152">You need hello object ID of hello user group toouse for managing hello resources.</span></span> <span data-ttu-id="7ab5f-153">Hello následující příklad ukazuje, jak tooget hello ID objektu ze skupiny hello zobrazovaný název:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-153">hello following example shows how tooget hello object ID from hello group's display name:</span></span>

```azurecli-interactive
az ad group show --group exampleGroupName
```

<span data-ttu-id="7ab5f-154">Hello Ukázkový příkaz vrátí hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-154">hello example command returns hello following output:</span></span>

```azurecli
{
    "displayName": "exampleGroupName",
    "mail": null,
    "objectId": "9aabd3ad-3716-4242-9d8e-a85df479d5d9",
    "objectType": "Group",
    "securityEnabled": true
}
```

<span data-ttu-id="7ab5f-155">tooretrieve jenom hello ID objektu, použijte:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-155">tooretrieve just hello object ID, use:</span></span>

```azurecli-interactive
groupid=$(az ad group show --group exampleGroupName --query objectId --output tsv)
```

## <a name="get-hello-role-definition-id"></a><span data-ttu-id="7ab5f-156">Získání ID definice role hello</span><span class="sxs-lookup"><span data-stu-id="7ab5f-156">Get hello role definition ID</span></span>

<span data-ttu-id="7ab5f-157">Dále je nutné zadat ID definice role hello hello předdefinovaná role RBAC chcete toogrant přístup toohello uživatele, skupiny uživatelů nebo aplikací.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-157">Next, you need hello role definition ID of hello RBAC built-in role you want toogrant access toohello user, user group, or application.</span></span> <span data-ttu-id="7ab5f-158">Obvykle použijete roli Přispěvatel nebo Čtenář nebo hello vlastníka.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-158">Typically, you use hello Owner or Contributor or Reader role.</span></span> <span data-ttu-id="7ab5f-159">Hello následující příkaz ukazuje, jak tooget hello ID definice role pro roli vlastníka hello:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-159">hello following command shows how tooget hello role definition ID for hello Owner role:</span></span>


```azurecli-interactive
az role definition list --name owner
```

<span data-ttu-id="7ab5f-160">Tento příkaz vrátí hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-160">That command returns hello following output:</span></span>

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

<span data-ttu-id="7ab5f-161">Je nutné hello hodnota vlastnosti "název" hello z předchozích příklad hello.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-161">You need hello value of hello "name" property from hello preceding example.</span></span> <span data-ttu-id="7ab5f-162">Můžete získat pouze vlastnosti s:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-162">You can retrieve just that property with:</span></span>

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

## <a name="create-hello-managed-application-definition"></a><span data-ttu-id="7ab5f-163">Vytvořit definici hello spravované aplikace</span><span class="sxs-lookup"><span data-stu-id="7ab5f-163">Create hello managed application definition</span></span>

<span data-ttu-id="7ab5f-164">Pokud již jste skupinu prostředků pro ukládání definice spravované aplikace, vytvořte jeden:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-164">If you do not already have a resource group for storing your managed application definition, create one now:</span></span>

```azurecli-interactive
az group create --name managedApplicationGroup --location westcentralus
```

<span data-ttu-id="7ab5f-165">Teď vytvořte hello spravované aplikace definice prostředků.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-165">Now, create hello managed application definition resource.</span></span>

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

<span data-ttu-id="7ab5f-166">jsou použité v předchozím příkladu hello Hello parametry:</span><span class="sxs-lookup"><span data-stu-id="7ab5f-166">hello parameters used in hello preceding example are:</span></span>

* <span data-ttu-id="7ab5f-167">**Skupina prostředků**: hello název skupiny prostředků hello které hello spravovaná definice aplikace se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-167">**resource-group**: hello name of hello resource group where hello managed application definition is created.</span></span>
* <span data-ttu-id="7ab5f-168">**Úroveň zámku**: hello typ zámku umístit do skupiny hello spravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-168">**lock-level**: hello type of lock placed on hello managed resource group.</span></span> <span data-ttu-id="7ab5f-169">Zákazník hello brání provádění nežádoucího operací v této skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-169">It prevents hello customer from performing undesirable operations on this resource group.</span></span> <span data-ttu-id="7ab5f-170">V současné době určené jen pro čtení je hello podporuje jenom úroveň zámku.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-170">Currently, ReadOnly is hello only supported lock level.</span></span> <span data-ttu-id="7ab5f-171">Pokud je zadána jen pro čtení, hello zákazníka můžete číst jenom hello prostředky ve skupině hello spravovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-171">When ReadOnly is specified, hello customer can only read hello resources present in hello managed resource group.</span></span>
* <span data-ttu-id="7ab5f-172">**povolení**: Popisuje hello ID objektu zabezpečení a hello ID definice role, které jsou používané toogrant oprávnění toohello spravovaných prostředků skupiny.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-172">**authorizations**: Describes hello principal ID and hello role definition ID that are used toogrant permission toohello managed resource group.</span></span> <span data-ttu-id="7ab5f-173">Jsou zadány ve formátu hello `<principalId>:<roleDefinitionId>`.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-173">It's specified in hello format of `<principalId>:<roleDefinitionId>`.</span></span> <span data-ttu-id="7ab5f-174">Více hodnot můžete zadat také pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-174">Multiple values also can be specified for this property.</span></span> <span data-ttu-id="7ab5f-175">Pokud je nutné více hodnot, musí být zadán v hello formuláře `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-175">If multiple values are needed, they should be specified in hello form `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`.</span></span> <span data-ttu-id="7ab5f-176">Více hodnot, jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-176">Multiple values are separated by a space.</span></span>
* <span data-ttu-id="7ab5f-177">**identifikátor uri balíčku souboru**: hello umístění balíčku hello spravované aplikace, který obsahuje soubory hello šablony, které může být objekt blob úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="7ab5f-177">**package-file-uri**: hello location of hello managed application package that contains hello template files, which can be an Azure Storage blob.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ab5f-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ab5f-178">Next steps</span></span>

* <span data-ttu-id="7ab5f-179">Úvod toomanaged aplikace naleznete v [spravované aplikace přehled](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7ab5f-179">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="7ab5f-180">Příklady hello souborů najdete v tématu [spravované aplikace ukázky](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="7ab5f-180">For examples of hello files, see [Managed application samples](https://github.com/Azure/azure-managedapp-samples/tree/master/samples).</span></span>
* <span data-ttu-id="7ab5f-181">Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="7ab5f-181">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
* <span data-ttu-id="7ab5f-182">Informace o publikování toohello spravovaných aplikací Azure Marketplace najdete v tématu [Azure spravované aplikace v hello Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="7ab5f-182">For information about publishing managed applications toohello Azure Marketplace, see [Azure managed applications in hello Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="7ab5f-183">Informace o použití spravovaných aplikací z hello Marketplace najdete v tématu [využívat Azure spravované aplikace v hello Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="7ab5f-183">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="7ab5f-184">jak toocreate soubor definice uživatelského rozhraní pro spravované aplikace, najdete v části toolearn [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7ab5f-184">toolearn how toocreate a UI definition file for a managed application, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
