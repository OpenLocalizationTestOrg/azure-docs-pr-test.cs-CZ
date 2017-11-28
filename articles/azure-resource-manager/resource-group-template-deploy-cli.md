---
title: "aaaDeploy prostředků pomocí Azure CLI a šablony | Microsoft Docs"
description: "Pomocí Azure Resource Manageru a rozhraní příkazového řádku Azure toodeploy tooAzure prostředky. Hello prostředky jsou definovány v šabloně Resource Manager."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 9f8bb9a8720399390a407030d2d32bcd97d32f13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a><span data-ttu-id="6c537-104">Nasazení prostředků pomocí šablon Resource Manageru a Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6c537-104">Deploy resources with Resource Manager templates and Azure CLI</span></span>

<span data-ttu-id="6c537-105">Toto téma vysvětluje, jak toouse 2.0 rozhraní příkazového řádku Azure s toodeploy šablony Resource Manageru tooAzure vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="6c537-105">This topic explains how toouse Azure CLI 2.0 with Resource Manager templates toodeploy your resources tooAzure.</span></span> <span data-ttu-id="6c537-106">Pokud nejste obeznámeni s hello koncepty nasazení a Správa řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6c537-106">If you are not familiar with hello concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>  

<span data-ttu-id="6c537-107">šablony Resource Manageru Hello nasadíte, může to být místní soubor na počítači, nebo externí soubor, který je umístěný v úložišti, jako je Githubu.</span><span class="sxs-lookup"><span data-stu-id="6c537-107">hello Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="6c537-108">Hello šablona nasazení v tomto článku je k dispozici v hello [Ukázka šablony](#sample-template) oddílu, nebo jako [Šablona účtu úložiště na webu GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="6c537-108">hello template you deploy in this article is available in hello [Sample template](#sample-template) section, or as a [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

<span data-ttu-id="6c537-109">Pokud nemáte nainstalované rozhraní příkazového řádku Azure, můžete použít hello [cloudové prostředí](#deploy-template-from-cloud-shell).</span><span class="sxs-lookup"><span data-stu-id="6c537-109">If you do not have Azure CLI installed, you can use hello [Cloud Shell](#deploy-template-from-cloud-shell).</span></span>

## <a name="deploy-local-template"></a><span data-ttu-id="6c537-110">Nasazení místního šablony</span><span class="sxs-lookup"><span data-stu-id="6c537-110">Deploy local template</span></span>

<span data-ttu-id="6c537-111">Při nasazování tooAzure prostředků, můžete:</span><span class="sxs-lookup"><span data-stu-id="6c537-111">When deploying resources tooAzure, you:</span></span>

1. <span data-ttu-id="6c537-112">Přihlaste se tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="6c537-112">Log in tooyour Azure account</span></span>
2. <span data-ttu-id="6c537-113">Vytvořte skupinu prostředků, která slouží jako kontejner hello hello nasazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="6c537-113">Create a resource group that serves as hello container for hello deployed resources.</span></span> <span data-ttu-id="6c537-114">Hello název hello skupiny prostředků může obsahovat jenom alfanumerické znaky, tečky, podtržítka, pomlčky a závorky.</span><span class="sxs-lookup"><span data-stu-id="6c537-114">hello name of hello resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="6c537-115">Může být až too90 znaků.</span><span class="sxs-lookup"><span data-stu-id="6c537-115">It can be up too90 characters.</span></span> <span data-ttu-id="6c537-116">Nemůže končit tečkou.</span><span class="sxs-lookup"><span data-stu-id="6c537-116">It cannot end in a period.</span></span>
3. <span data-ttu-id="6c537-117">Nasazení toohello prostředků skupiny hello šablony, který definuje prostředky toocreate hello</span><span class="sxs-lookup"><span data-stu-id="6c537-117">Deploy toohello resource group hello template that defines hello resources toocreate</span></span>

<span data-ttu-id="6c537-118">Šablona může obsahovat parametry, které umožňují toocustomize hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="6c537-118">A template can include parameters that enable you toocustomize hello deployment.</span></span> <span data-ttu-id="6c537-119">Například můžete zadat hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním).</span><span class="sxs-lookup"><span data-stu-id="6c537-119">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="6c537-120">Ukázka šablony Hello definuje parametr pro účet úložiště hello SKU.</span><span class="sxs-lookup"><span data-stu-id="6c537-120">hello sample template defines a parameter for hello storage account SKU.</span></span> 

<span data-ttu-id="6c537-121">Hello následující ukázka vytvoří skupinu prostředků a nasadí šablonu z místního počítače:</span><span class="sxs-lookup"><span data-stu-id="6c537-121">hello following example creates a resource group, and deploys a template from your local machine:</span></span>

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="6c537-122">Hello nasazení může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6c537-122">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="6c537-123">Po dokončení zobrazí zprávu, která zahrnuje hello výsledek:</span><span class="sxs-lookup"><span data-stu-id="6c537-123">When it finishes, you see a message that includes hello result:</span></span>

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a><span data-ttu-id="6c537-124">Nasazení externí šablony</span><span class="sxs-lookup"><span data-stu-id="6c537-124">Deploy external template</span></span>

<span data-ttu-id="6c537-125">Místo uložení šablony Resource Manageru na místním počítači, dáte možná přednost toostore je v externí umístění.</span><span class="sxs-lookup"><span data-stu-id="6c537-125">Instead of storing Resource Manager templates on your local machine, you may prefer toostore them in an external location.</span></span> <span data-ttu-id="6c537-126">Šablony můžete uložit ve úložiště řízení zdrojů (například Githubu).</span><span class="sxs-lookup"><span data-stu-id="6c537-126">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="6c537-127">Nebo byste je uložit v účtu úložiště Azure pro přístup ke sdílenému ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="6c537-127">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="6c537-128">toodeploy šablonu externí použít hello **šablony uri** parametr.</span><span class="sxs-lookup"><span data-stu-id="6c537-128">toodeploy an external template, use hello **template-uri** parameter.</span></span> <span data-ttu-id="6c537-129">Použijte hello URI v hello příklad toodeploy hello Ukázka šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="6c537-129">Use hello URI in hello example toodeploy hello sample template from GitHub.</span></span>
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="6c537-130">Hello předchozí příklad vyžaduje veřejně přístupná identifikátor URI pro hello šablony, která funguje pro většinu scénářů, protože vaše šablona by neměla zahrnovat citlivá data.</span><span class="sxs-lookup"><span data-stu-id="6c537-130">hello preceding example requires a publicly accessible URI for hello template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="6c537-131">Pokud potřebujete toospecify citlivá data (např. heslo správce), předejte jako parametr zabezpečené tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6c537-131">If you need toospecify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="6c537-132">Ale pokud nechcete, aby vaše šablona toobe veřejně přístupný, můžete chránit ho ukládáním do kontejner privátní úložiště.</span><span class="sxs-lookup"><span data-stu-id="6c537-132">However, if you do not want your template toobe publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="6c537-133">Informace o nasazení šablony, která vyžaduje token sdílený přístupový podpis (SAS) najdete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="6c537-133">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="6c537-134">Nasazení šablony ze služby Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="6c537-134">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="6c537-135">Můžete použít [cloudové prostředí](../cloud-shell/overview.md) toorun hello rozhraní příkazového řádku Azure příkazy pro nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="6c537-135">You can use [Cloud Shell](../cloud-shell/overview.md) toorun hello Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="6c537-136">Ale je nutné nejdřív načíst šablony do hello sdílené složky pro vaše cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="6c537-136">However, you must first load your template into hello file share for your Cloud Shell.</span></span> <span data-ttu-id="6c537-137">Pokud jste ještě službu Cloud Shell nepoužívali, přečtěte si téma [Přehled služby Azure Cloud Shell](../cloud-shell/overview.md), kde najdete informace o jejím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6c537-137">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="6c537-138">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6c537-138">Log in toohello [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="6c537-139">Vyberte vaši skupinu prostředků služby Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6c537-139">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="6c537-140">vzor názvů Hello je `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="6c537-140">hello name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Výběr skupiny prostředků](./media/resource-group-template-deploy-cli/select-cs-resource-group.png)

3. <span data-ttu-id="6c537-142">Vyberte účet úložiště hello pro své cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="6c537-142">Select hello storage account for your Cloud Shell.</span></span>

   ![Výběr účtu úložiště](./media/resource-group-template-deploy-cli/select-storage.png)

4. <span data-ttu-id="6c537-144">Vyberte **Soubory**.</span><span class="sxs-lookup"><span data-stu-id="6c537-144">Select **Files**.</span></span>

   ![Výběr souborů](./media/resource-group-template-deploy-cli/select-files.png)

5. <span data-ttu-id="6c537-146">Vyberte hello sdílené složky pro cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="6c537-146">Select hello file share for Cloud Shell.</span></span> <span data-ttu-id="6c537-147">vzor názvů Hello je `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="6c537-147">hello name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Výběr sdílené složky](./media/resource-group-template-deploy-cli/select-file-share.png)

6. <span data-ttu-id="6c537-149">Vyberte **Přidat adresář**.</span><span class="sxs-lookup"><span data-stu-id="6c537-149">Select **Add directory**.</span></span>

   ![Přidání adresáře](./media/resource-group-template-deploy-cli/select-add-directory.png)

7. <span data-ttu-id="6c537-151">Pojmenujte ho **templates** a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c537-151">Name it **templates**, and select **Okay**.</span></span>

   ![Pojmenování adresáře](./media/resource-group-template-deploy-cli/name-templates.png)

8. <span data-ttu-id="6c537-153">Vyberte nový adresář.</span><span class="sxs-lookup"><span data-stu-id="6c537-153">Select your new directory.</span></span>

   ![Výběr adresáře](./media/resource-group-template-deploy-cli/select-templates.png)

9. <span data-ttu-id="6c537-155">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="6c537-155">Select **Upload**.</span></span>

   ![Výběr nahrání](./media/resource-group-template-deploy-cli/select-upload.png)

10. <span data-ttu-id="6c537-157">Vyhledejte a nahrajte vaši šablonu.</span><span class="sxs-lookup"><span data-stu-id="6c537-157">Find and upload your template.</span></span>

   ![Nahrání souboru](./media/resource-group-template-deploy-cli/upload-files.png)

11. <span data-ttu-id="6c537-159">Otevřete hello řádku.</span><span class="sxs-lookup"><span data-stu-id="6c537-159">Open hello prompt.</span></span>

   ![Otevření služby Cloud Shell](./media/resource-group-template-deploy-cli/start-cloud-shell.png)

12. <span data-ttu-id="6c537-161">Zadejte následující příkazy v prostředí cloudu hello hello:</span><span class="sxs-lookup"><span data-stu-id="6c537-161">Enter hello following commands in hello Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a><span data-ttu-id="6c537-162">Soubory parametrů</span><span class="sxs-lookup"><span data-stu-id="6c537-162">Parameter files</span></span>

<span data-ttu-id="6c537-163">Místo předávání parametrů jako vložené hodnoty ve vašem skriptu, možná bude snazší toouse soubor JSON, který obsahuje hodnoty parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="6c537-163">Rather than passing parameters as inline values in your script, you may find it easier toouse a JSON file that contains hello parameter values.</span></span> <span data-ttu-id="6c537-164">soubor parametrů Hello musí být ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="6c537-164">hello parameter file must be in hello following format:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

<span data-ttu-id="6c537-165">Všimněte si, že část parametry hello obsahuje název parametru, který odpovídá hello parametr definovaný v šabloně (storageAccountType).</span><span class="sxs-lookup"><span data-stu-id="6c537-165">Notice that hello parameters section includes a parameter name that matches hello parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="6c537-166">Hello soubor parametrů obsahuje hodnotu pro parametr hello.</span><span class="sxs-lookup"><span data-stu-id="6c537-166">hello parameter file contains a value for hello parameter.</span></span> <span data-ttu-id="6c537-167">Tato hodnota je předán toohello šablony automaticky během nasazení.</span><span class="sxs-lookup"><span data-stu-id="6c537-167">This value is automatically passed toohello template during deployment.</span></span> <span data-ttu-id="6c537-168">Můžete vytvořit několik souborů parametr pro různé scénáře nasazení a pak předejte soubor hello odpovídající parametr.</span><span class="sxs-lookup"><span data-stu-id="6c537-168">You can create multiple parameter files for different deployment scenarios, and then pass in hello appropriate parameter file.</span></span> 

<span data-ttu-id="6c537-169">Zkopírujte hello předcházející příklad a uložte ho jako soubor s názvem `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="6c537-169">Copy hello preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="6c537-170">použít toopass soubor místní parametru `@` toospecify místní soubor s názvem storage.parameters.json.</span><span class="sxs-lookup"><span data-stu-id="6c537-170">toopass a local parameter file, use `@` toospecify a local file named storage.parameters.json.</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a><span data-ttu-id="6c537-171">Testovací nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="6c537-171">Test a template deployment</span></span>

<span data-ttu-id="6c537-172">pomocí šablony a parametr hodnoty bez ve skutečnosti nasazení všechny prostředky tootest [ověření nasazení skupiny az](/cli/azure/group/deployment#validate).</span><span class="sxs-lookup"><span data-stu-id="6c537-172">tootest your template and parameter values without actually deploying any resources, use [az group deployment validate](/cli/azure/group/deployment#validate).</span></span> 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

<span data-ttu-id="6c537-173">Pokud nejsou zjištěny žádné chyby, hello příkaz vrátí informace o hello testovací nasazení.</span><span class="sxs-lookup"><span data-stu-id="6c537-173">If no errors are detected, hello command returns information about hello test deployment.</span></span> <span data-ttu-id="6c537-174">Konkrétně, Všimněte si, že hello **chyba** hodnota je null.</span><span class="sxs-lookup"><span data-stu-id="6c537-174">In particular, notice that hello **error** value is null.</span></span>

```azurecli
{
  "error": null,
  "properties": {
      ...
```

<span data-ttu-id="6c537-175">Pokud dojde k chybě, hello příkaz vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="6c537-175">If an error is detected, hello command returns an error message.</span></span> <span data-ttu-id="6c537-176">Například pokus toopass nesprávná hodnota pro účet úložiště hello SKU, vrátí hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="6c537-176">For example, attempting toopass an incorrect value for hello storage account SKU, returns hello following error:</span></span>

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'hello provided value 'badSKU' for hello template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. hello parameter value is not part of hello allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

<span data-ttu-id="6c537-177">Pokud vaše šablona obsahuje chybu syntaxe, příkaz hello chybovou zprávu oznamující, že ho nebylo možné rozložit hello šablony.</span><span class="sxs-lookup"><span data-stu-id="6c537-177">If your template has a syntax error, hello command returns an error indicating it could not parse hello template.</span></span> <span data-ttu-id="6c537-178">uvítací zprávu označuje číslo řádku hello a pozice hello při analýze.</span><span class="sxs-lookup"><span data-stu-id="6c537-178">hello message indicates hello line number and position of hello parsing error.</span></span>

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="6c537-179">režim dokončení toouse, použijte hello `mode` parametr:</span><span class="sxs-lookup"><span data-stu-id="6c537-179">toouse complete mode, use hello `mode` parameter:</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a><span data-ttu-id="6c537-180">Ukázka šablony</span><span class="sxs-lookup"><span data-stu-id="6c537-180">Sample template</span></span>

<span data-ttu-id="6c537-181">Hello následující šablona se používá pro hello příklady v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="6c537-181">hello following template is used for hello examples in this topic.</span></span> <span data-ttu-id="6c537-182">Zkopírujte a uložte ho jako soubor s názvem storage.json.</span><span class="sxs-lookup"><span data-stu-id="6c537-182">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="6c537-183">toounderstand jak toocreate této šablony najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="6c537-183">toounderstand how toocreate this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="6c537-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c537-184">Next steps</span></span>
* <span data-ttu-id="6c537-185">Hello příklady v tomto článku nasazení skupiny prostředků tooa prostředky ve vašem předplatném výchozí.</span><span class="sxs-lookup"><span data-stu-id="6c537-185">hello examples in this article deploy resources tooa resource group in your default subscription.</span></span> <span data-ttu-id="6c537-186">toouse jiného předplatného, najdete v části [spravovat víc předplatných Azure](/cli/azure/manage-azure-subscriptions-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6c537-186">toouse a different subscription, see [Manage multiple Azure subscriptions](/cli/azure/manage-azure-subscriptions-azure-cli).</span></span>
* <span data-ttu-id="6c537-187">Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript nasazení šablony Resource Manageru](resource-manager-samples-cli-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="6c537-187">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-cli-deploy.md).</span></span>
* <span data-ttu-id="6c537-188">jak zjistit, toodefine parametry v šabloně, toounderstand [pochopit strukturu hello a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6c537-188">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="6c537-189">Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="6c537-189">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="6c537-190">Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="6c537-190">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>
* <span data-ttu-id="6c537-191">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6c537-191">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
