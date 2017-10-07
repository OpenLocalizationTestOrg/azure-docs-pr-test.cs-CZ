---
title: "aaaCreate první šablony Azure Resource Manageru | Microsoft Docs"
description: "Podrobný průvodce toocreating první šablony Azure Resource Manager. Se dozvíte, jak toouse hello odkaz na šablonu pro šablonu hello toocreate účet úložiště."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/27/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.openlocfilehash: 92e6d6bb7094fe0e4537ee080704967862804bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a><span data-ttu-id="09795-104">Vytvoření a nasazení první šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="09795-104">Create and deploy your first Azure Resource Manager template</span></span>
<span data-ttu-id="09795-105">Toto téma vás provede kroky hello k vytvoření vaší první šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="09795-105">This topic walks you through hello steps of creating your first Azure Resource Manager template.</span></span> <span data-ttu-id="09795-106">Šablony Resource Manageru jsou soubory JSON, které definují hello prostředky, které potřebujete toodeploy pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="09795-106">Resource Manager templates are JSON files that define hello resources you need toodeploy for your solution.</span></span> <span data-ttu-id="09795-107">Koncepty hello toounderstand přidružené k nasazení a správě řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="09795-107">toounderstand hello concepts associated with deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span> <span data-ttu-id="09795-108">Pokud máte existující prostředky a chcete tooget šablonu pro tyto prostředky, přečtěte si téma [Export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="09795-108">If you have existing resources and want tooget a template for those resources, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

<span data-ttu-id="09795-109">toocreate a opravit šablony, je nutné JSON editor.</span><span class="sxs-lookup"><span data-stu-id="09795-109">toocreate and revise templates, you need a JSON editor.</span></span> <span data-ttu-id="09795-110">[Visual Studio Code](https://code.visualstudio.com/) je jednoduchý, opensourcový editor kódu pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="09795-110">[Visual Studio Code](https://code.visualstudio.com/) is a lightweight, open-source, cross-platform code editor.</span></span> <span data-ttu-id="09795-111">Důrazně doporučujeme k vytváření šablon Resource Manageru používat Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="09795-111">We strongly recommend using Visual Studio Code for creating Resource Manager templates.</span></span> <span data-ttu-id="09795-112">Toto téma předpokládá, že používáte VS Code, pokud však máte jiný editor JSON (například sadu Visual Studio), můžete použít ten.</span><span class="sxs-lookup"><span data-stu-id="09795-112">This topic assumes you are using VS Code; however, if you have another JSON editor (like Visual Studio), you can use that editor.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09795-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="09795-113">Prerequisites</span></span>

* <span data-ttu-id="09795-114">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="09795-114">Visual Studio Code.</span></span> <span data-ttu-id="09795-115">V případě potřeby ho nainstalujte z adresy [https://code.visualstudio.com/](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="09795-115">If needed, install it from [https://code.visualstudio.com/](https://code.visualstudio.com/).</span></span>
* <span data-ttu-id="09795-116">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="09795-116">An Azure subscription.</span></span> <span data-ttu-id="09795-117">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="09795-117">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="create-template"></a><span data-ttu-id="09795-118">Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="09795-118">Create template</span></span>

<span data-ttu-id="09795-119">Začněme jednoduché šablonu, která nasadí tooyour předplatné účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="09795-119">Let's start with a simple template that deploys a storage account tooyour subscription.</span></span>

1. <span data-ttu-id="09795-120">Vyberte **Soubor** > **Nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="09795-120">Select **File** > **New File**.</span></span> 

   ![Nový soubor](./media/resource-manager-create-first-template/new-file.png)

2. <span data-ttu-id="09795-122">Zkopírujte a vložte následující syntaxe JSON do souboru hello:</span><span class="sxs-lookup"><span data-stu-id="09795-122">Copy and paste hello following JSON syntax into your file:</span></span>

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   <span data-ttu-id="09795-123">Názvy účtů úložiště mají několik omezení, díky kterým tooset obtížná.</span><span class="sxs-lookup"><span data-stu-id="09795-123">Storage account names have several restrictions that make them difficult tooset.</span></span> <span data-ttu-id="09795-124">Název Hello musí být v rozmezí 3 až 24 znaků délku, použijte pouze čísla a malá písmena a musí být jedinečná.</span><span class="sxs-lookup"><span data-stu-id="09795-124">hello name must be between 3 and 24 characters in length, use only numbers and lower-case letters, and be unique.</span></span> <span data-ttu-id="09795-125">Hello předchozí šablona používá hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce toogenerate hodnotu hash.</span><span class="sxs-lookup"><span data-stu-id="09795-125">hello preceding template uses hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function toogenerate a hash value.</span></span> <span data-ttu-id="09795-126">toogive tato hodnota hash hodnoty více znamená, přidá hello předponu *úložiště*.</span><span class="sxs-lookup"><span data-stu-id="09795-126">toogive this hash value more meaning, it adds hello prefix *storage*.</span></span> 

3. <span data-ttu-id="09795-127">Uložte tento soubor jako **azuredeploy.json** tooa místní složky.</span><span class="sxs-lookup"><span data-stu-id="09795-127">Save this file as **azuredeploy.json** tooa local folder.</span></span>

   ![Uložení šablony](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a><span data-ttu-id="09795-129">Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="09795-129">Deploy template</span></span>

<span data-ttu-id="09795-130">Můžete je připraven toodeploy této šablony.</span><span class="sxs-lookup"><span data-stu-id="09795-130">You are ready toodeploy this template.</span></span> <span data-ttu-id="09795-131">Můžete použít PowerShell nebo rozhraní příkazového řádku Azure toocreate skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="09795-131">You use either PowerShell or Azure CLI toocreate a resource group.</span></span> <span data-ttu-id="09795-132">Potom nasadíte skupinu prostředků toothat účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="09795-132">Then, you deploy a storage account toothat resource group.</span></span>

* <span data-ttu-id="09795-133">Pro prostředí PowerShell použijte následující příkazy z hello složku obsahující hello šablony hello:</span><span class="sxs-lookup"><span data-stu-id="09795-133">For PowerShell, use hello following commands from hello folder containing hello template:</span></span>

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* <span data-ttu-id="09795-134">Pro místní instalaci rozhraní příkazového řádku Azure použijte následující příkazy z hello složku obsahující hello šablony hello:</span><span class="sxs-lookup"><span data-stu-id="09795-134">For a local installation of Azure CLI, use hello following commands from hello folder containing hello template:</span></span>

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

<span data-ttu-id="09795-135">Po dokončení nasazení účtu úložiště existuje ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="09795-135">When deployment finishes, your storage account exists in hello resource group.</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="09795-136">Nasazení šablony ze služby Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="09795-136">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="09795-137">Můžete použít [cloudové prostředí](../cloud-shell/overview.md) toorun hello rozhraní příkazového řádku Azure příkazy pro nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="09795-137">You can use [Cloud Shell](../cloud-shell/overview.md) toorun hello Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="09795-138">Ale je nutné nejdřív načíst šablony do hello sdílené složky pro vaše cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="09795-138">However, you must first load your template into hello file share for your Cloud Shell.</span></span> <span data-ttu-id="09795-139">Pokud jste ještě službu Cloud Shell nepoužívali, přečtěte si téma [Přehled služby Azure Cloud Shell](../cloud-shell/overview.md), kde najdete informace o jejím nastavení.</span><span class="sxs-lookup"><span data-stu-id="09795-139">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="09795-140">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="09795-140">Log in toohello [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="09795-141">Vyberte vaši skupinu prostředků služby Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="09795-141">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="09795-142">vzor názvů Hello je `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="09795-142">hello name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Výběr skupiny prostředků](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. <span data-ttu-id="09795-144">Vyberte účet úložiště hello pro své cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="09795-144">Select hello storage account for your Cloud Shell.</span></span>

   ![Výběr účtu úložiště](./media/resource-manager-create-first-template/select-storage.png)

4. <span data-ttu-id="09795-146">Vyberte **Soubory**.</span><span class="sxs-lookup"><span data-stu-id="09795-146">Select **Files**.</span></span>

   ![Výběr souborů](./media/resource-manager-create-first-template/select-files.png)

5. <span data-ttu-id="09795-148">Vyberte hello sdílené složky pro cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="09795-148">Select hello file share for Cloud Shell.</span></span> <span data-ttu-id="09795-149">vzor názvů Hello je `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="09795-149">hello name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Výběr sdílené složky](./media/resource-manager-create-first-template/select-file-share.png)

6. <span data-ttu-id="09795-151">Vyberte **Přidat adresář**.</span><span class="sxs-lookup"><span data-stu-id="09795-151">Select **Add directory**.</span></span>

   ![Přidání adresáře](./media/resource-manager-create-first-template/select-add-directory.png)

7. <span data-ttu-id="09795-153">Pojmenujte ho **templates** a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="09795-153">Name it **templates**, and select **Okay**.</span></span>

   ![Pojmenování adresáře](./media/resource-manager-create-first-template/name-templates.png)

8. <span data-ttu-id="09795-155">Vyberte nový adresář.</span><span class="sxs-lookup"><span data-stu-id="09795-155">Select your new directory.</span></span>

   ![Výběr adresáře](./media/resource-manager-create-first-template/select-templates.png)

9. <span data-ttu-id="09795-157">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="09795-157">Select **Upload**.</span></span>

   ![Výběr nahrání](./media/resource-manager-create-first-template/select-upload.png)

10. <span data-ttu-id="09795-159">Vyhledejte a nahrajte vaši šablonu.</span><span class="sxs-lookup"><span data-stu-id="09795-159">Find and upload your template.</span></span>

   ![Nahrání souboru](./media/resource-manager-create-first-template/upload-files.png)

11. <span data-ttu-id="09795-161">Otevřete hello řádku.</span><span class="sxs-lookup"><span data-stu-id="09795-161">Open hello prompt.</span></span>

   ![Otevření služby Cloud Shell](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. <span data-ttu-id="09795-163">Zadejte následující příkazy v prostředí cloudu hello hello:</span><span class="sxs-lookup"><span data-stu-id="09795-163">Enter hello following commands in hello Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

<span data-ttu-id="09795-164">Po dokončení nasazení účtu úložiště existuje ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="09795-164">When deployment finishes, your storage account exists in hello resource group.</span></span>

## <a name="customize-hello-template"></a><span data-ttu-id="09795-165">Přizpůsobení šablony hello</span><span class="sxs-lookup"><span data-stu-id="09795-165">Customize hello template</span></span>

<span data-ttu-id="09795-166">Šablona Hello funguje bez problémů, ale není flexibilní.</span><span class="sxs-lookup"><span data-stu-id="09795-166">hello template works fine, but it is not flexible.</span></span> <span data-ttu-id="09795-167">Nasadí vždy místně redundantního úložiště tooSouth střed USA.</span><span class="sxs-lookup"><span data-stu-id="09795-167">It always deploys a locally redundant storage tooSouth Central US.</span></span> <span data-ttu-id="09795-168">Název Hello je vždy *úložiště* následuje hodnotu hash.</span><span class="sxs-lookup"><span data-stu-id="09795-168">hello name is always *storage* followed by a hash value.</span></span> <span data-ttu-id="09795-169">tooenable pomocí hello šablony pro různé scénáře, přidání šablony toohello parametry.</span><span class="sxs-lookup"><span data-stu-id="09795-169">tooenable using hello template for different scenarios, add parameters toohello template.</span></span>

<span data-ttu-id="09795-170">Hello následující příklad ukazuje hello parametry části s dva parametry.</span><span class="sxs-lookup"><span data-stu-id="09795-170">hello following example shows hello parameters section with two parameters.</span></span> <span data-ttu-id="09795-171">první parametr Hello `storageSKU` vám umožní toospecify hello typ redundance.</span><span class="sxs-lookup"><span data-stu-id="09795-171">hello first parameter `storageSKU` enables you toospecify hello type of redundancy.</span></span> <span data-ttu-id="09795-172">Omezuje hello hodnoty, které lze předat v toovalues, které jsou platné pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="09795-172">It limits hello values you can pass in toovalues that are valid for a storage account.</span></span> <span data-ttu-id="09795-173">Také určuje výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="09795-173">It also specifies a default value.</span></span> <span data-ttu-id="09795-174">druhý parametr Hello `storageNamePrefix` je sada tooallow delší než 11 znaků.</span><span class="sxs-lookup"><span data-stu-id="09795-174">hello second parameter `storageNamePrefix` is set tooallow a maximum of 11 characters.</span></span> <span data-ttu-id="09795-175">Určuje výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="09795-175">It specifies a default value.</span></span>

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "hello type of replication toouse for hello storage account."
    }
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

<span data-ttu-id="09795-176">V části proměnných hello přidat proměnné s názvem `storageName`.</span><span class="sxs-lookup"><span data-stu-id="09795-176">In hello variables section, add a variable named `storageName`.</span></span> <span data-ttu-id="09795-177">Spojuje v sobě hello předpona hodnoty z parametrů hello a hodnotu hash z hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce.</span><span class="sxs-lookup"><span data-stu-id="09795-177">It combines hello prefix value from hello parameters and a hash value from hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function.</span></span> <span data-ttu-id="09795-178">Používá hello [toLower](resource-group-template-functions-string.md#tolower) funkce tooconvert toolowercase všechny znaky.</span><span class="sxs-lookup"><span data-stu-id="09795-178">It uses hello [toLower](resource-group-template-functions-string.md#tolower) function tooconvert all characters toolowercase.</span></span>

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

<span data-ttu-id="09795-179">toouse tyto nové hodnoty pro váš účet úložiště, změňte definici prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="09795-179">toouse these new values for your storage account, change hello resource definition:</span></span>

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    "kind": "Storage",
    "location": "[resourceGroup().location]",
    "tags": {},
    "properties": {}
  }
],
```

<span data-ttu-id="09795-180">Všimněte si, že hello název účtu úložiště hello je teď nastavená toohello proměnnou, kterou jste přidali.</span><span class="sxs-lookup"><span data-stu-id="09795-180">Notice that hello name of hello storage account is now set toohello variable that you added.</span></span> <span data-ttu-id="09795-181">Název SKU Hello se nastaví hodnota toohello parametru hello.</span><span class="sxs-lookup"><span data-stu-id="09795-181">hello SKU name is set toohello value of hello parameter.</span></span> <span data-ttu-id="09795-182">Hello umístění se nastaví hello stejné umístění jako skupina prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="09795-182">hello location is set hello same location as hello resource group.</span></span>

<span data-ttu-id="09795-183">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="09795-183">Save your file.</span></span> 

<span data-ttu-id="09795-184">Po dokončení kroků hello v tomto článku, vaše šablona teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="09795-184">After completing hello steps in this article, your template now looks like:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "hello type of replication toouse for hello storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a><span data-ttu-id="09795-185">Opětovné nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="09795-185">Redeploy template</span></span>

<span data-ttu-id="09795-186">Znovu nasaďte šablonu hello s různými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="09795-186">Redeploy hello template with different values.</span></span>

<span data-ttu-id="09795-187">Pokud používáte PowerShell, použijte:</span><span class="sxs-lookup"><span data-stu-id="09795-187">For PowerShell, use:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

<span data-ttu-id="09795-188">Pokud používáte Azure CLI, použijte:</span><span class="sxs-lookup"><span data-stu-id="09795-188">For Azure CLI, use:</span></span>

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

<span data-ttu-id="09795-189">Hello cloudové prostředí nahrajte změněné šablony toohello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="09795-189">For hello Cloud Shell, upload your changed template toohello file share.</span></span> <span data-ttu-id="09795-190">Přepište stávající soubor hello.</span><span class="sxs-lookup"><span data-stu-id="09795-190">Overwrite hello existing file.</span></span> <span data-ttu-id="09795-191">Pak použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="09795-191">Then, use hello following command:</span></span>

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="clean-up-resources"></a><span data-ttu-id="09795-192">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="09795-192">Clean up resources</span></span>

<span data-ttu-id="09795-193">Pokud již nepotřebujete, vyčistěte hello prostředky, které jste nasadili odstraněním skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="09795-193">When no longer needed, clean up hello resources you deployed by deleting hello resource group.</span></span>

<span data-ttu-id="09795-194">Pokud používáte PowerShell, použijte:</span><span class="sxs-lookup"><span data-stu-id="09795-194">For PowerShell, use:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

<span data-ttu-id="09795-195">Pokud používáte Azure CLI, použijte:</span><span class="sxs-lookup"><span data-stu-id="09795-195">For Azure CLI, use:</span></span>

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a><span data-ttu-id="09795-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09795-196">Next steps</span></span>
* <span data-ttu-id="09795-197">toolearn Další informace o struktuře hello šablon, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="09795-197">toolearn more about hello structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="09795-198">toolearn o hello vlastnosti pro účet úložiště, najdete v části [odkaz na šablonu účtů úložiště](/azure/templates/microsoft.storage/storageaccounts).</span><span class="sxs-lookup"><span data-stu-id="09795-198">toolearn about hello properties for a storage account, see [storage accounts template reference](/azure/templates/microsoft.storage/storageaccounts).</span></span>
* <span data-ttu-id="09795-199">tooview dokončení šablon pro mnoho různých typů řešení, najdete v části hello [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="09795-199">tooview complete templates for many different types of solutions, see hello [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
