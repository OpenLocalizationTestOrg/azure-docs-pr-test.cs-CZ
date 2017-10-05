---
title: "Nasazení prostředků pomocí Azure CLI a šablony | Microsoft Docs"
description: "Nasazení prostředky do Azure pomocí Azure Resource Manageru a rozhraní příkazového řádku Azure. Prostředky jsou definovány v šabloně Resource Manageru."
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
ms.openlocfilehash: 4f1d5f4cc48470f8906edb28628006dd1996bd3a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a><span data-ttu-id="6a250-104">Nasazení prostředků pomocí šablon Resource Manageru a Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6a250-104">Deploy resources with Resource Manager templates and Azure CLI</span></span>

<span data-ttu-id="6a250-105">Toto téma vysvětluje, jak pomocí Azure CLI 2.0 šablony Resource Manageru k nasazení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6a250-105">This topic explains how to use Azure CLI 2.0 with Resource Manager templates to deploy your resources to Azure.</span></span> <span data-ttu-id="6a250-106">Pokud nejste obeznámeni s koncepty nasazení a Správa řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-106">If you are not familiar with the concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>  

<span data-ttu-id="6a250-107">Šablony Resource Manageru, který nasazujete, může to být místní soubor na počítači, nebo externí soubor, který je umístěný v úložišti, jako je Githubu.</span><span class="sxs-lookup"><span data-stu-id="6a250-107">The Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="6a250-108">Šablona nasazení v tomto článku je k dispozici v [vzorové šablony](#sample-template) oddílu, nebo jako [Šablona účtu úložiště na webu GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="6a250-108">The template you deploy in this article is available in the [Sample template](#sample-template) section, or as a [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

<span data-ttu-id="6a250-109">Pokud nemáte nainstalované rozhraní příkazového řádku Azure, můžete použít [cloudové prostředí](#deploy-template-from-cloud-shell).</span><span class="sxs-lookup"><span data-stu-id="6a250-109">If you do not have Azure CLI installed, you can use the [Cloud Shell](#deploy-template-from-cloud-shell).</span></span>

## <a name="deploy-local-template"></a><span data-ttu-id="6a250-110">Nasazení místního šablony</span><span class="sxs-lookup"><span data-stu-id="6a250-110">Deploy local template</span></span>

<span data-ttu-id="6a250-111">Při nasazování prostředků do Azure, můžete:</span><span class="sxs-lookup"><span data-stu-id="6a250-111">When deploying resources to Azure, you:</span></span>

1. <span data-ttu-id="6a250-112">Přihlaste se k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="6a250-112">Log in to your Azure account</span></span>
2. <span data-ttu-id="6a250-113">Vytvořte skupinu prostředků, která slouží jako kontejner pro nasazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="6a250-113">Create a resource group that serves as the container for the deployed resources.</span></span> <span data-ttu-id="6a250-114">Název skupiny prostředků může obsahovat pouze alfanumerické znaky, tečky, podtržítka, pomlčky a závorky.</span><span class="sxs-lookup"><span data-stu-id="6a250-114">The name of the resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="6a250-115">Může být až 90 znaků.</span><span class="sxs-lookup"><span data-stu-id="6a250-115">It can be up to 90 characters.</span></span> <span data-ttu-id="6a250-116">Nemůže končit tečkou.</span><span class="sxs-lookup"><span data-stu-id="6a250-116">It cannot end in a period.</span></span>
3. <span data-ttu-id="6a250-117">Nasazení do skupiny prostředků definující zdrojů pro vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="6a250-117">Deploy to the resource group the template that defines the resources to create</span></span>

<span data-ttu-id="6a250-118">Šablonu může obsahovat parametry, které vám umožní přizpůsobit nasazení.</span><span class="sxs-lookup"><span data-stu-id="6a250-118">A template can include parameters that enable you to customize the deployment.</span></span> <span data-ttu-id="6a250-119">Například můžete zadat hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním).</span><span class="sxs-lookup"><span data-stu-id="6a250-119">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="6a250-120">Ukázka šablony definuje parametr pro účet úložiště SKU.</span><span class="sxs-lookup"><span data-stu-id="6a250-120">The sample template defines a parameter for the storage account SKU.</span></span> 

<span data-ttu-id="6a250-121">Následující příklad vytvoří skupinu prostředků a nasadí šablonu z místního počítače:</span><span class="sxs-lookup"><span data-stu-id="6a250-121">The following example creates a resource group, and deploys a template from your local machine:</span></span>

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="6a250-122">Dokončení nasazení může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="6a250-122">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="6a250-123">Po dokončení zobrazí zprávu, která obsahuje výsledek:</span><span class="sxs-lookup"><span data-stu-id="6a250-123">When it finishes, you see a message that includes the result:</span></span>

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a><span data-ttu-id="6a250-124">Nasazení externí šablony</span><span class="sxs-lookup"><span data-stu-id="6a250-124">Deploy external template</span></span>

<span data-ttu-id="6a250-125">Místo uložení šablony Resource Manageru na místním počítači, dáváte přednost uložit je do externího umístění.</span><span class="sxs-lookup"><span data-stu-id="6a250-125">Instead of storing Resource Manager templates on your local machine, you may prefer to store them in an external location.</span></span> <span data-ttu-id="6a250-126">Šablony můžete uložit ve úložiště řízení zdrojů (například Githubu).</span><span class="sxs-lookup"><span data-stu-id="6a250-126">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="6a250-127">Nebo byste je uložit v účtu úložiště Azure pro přístup ke sdílenému ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="6a250-127">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="6a250-128">Chcete-li nasadit externí šablonu, použijte **šablony uri** parametr.</span><span class="sxs-lookup"><span data-stu-id="6a250-128">To deploy an external template, use the **template-uri** parameter.</span></span> <span data-ttu-id="6a250-129">Identifikátor URI v příkladu použijte k nasazení ukázkové šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="6a250-129">Use the URI in the example to deploy the sample template from GitHub.</span></span>
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="6a250-130">V předchozím příkladu vyžaduje veřejně přístupná identifikátor URI pro šablony, která funguje pro většinu scénářů, protože vaše šablona by neměla zahrnovat citlivá data.</span><span class="sxs-lookup"><span data-stu-id="6a250-130">The preceding example requires a publicly accessible URI for the template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="6a250-131">Pokud budete muset zadat citlivá data (např. heslo správce), předejte jako parametr zabezpečené tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6a250-131">If you need to specify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="6a250-132">Ale pokud nechcete, aby vaše šablona veřejně přístupný, můžete chránit jeho uložením v kontejneru privátní úložiště.</span><span class="sxs-lookup"><span data-stu-id="6a250-132">However, if you do not want your template to be publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="6a250-133">Informace o nasazení šablony, která vyžaduje token sdílený přístupový podpis (SAS) najdete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-133">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="6a250-134">Nasazení šablony ze služby Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="6a250-134">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="6a250-135">Ke spuštění příkazů Azure CLI pro nasazení šablony můžete použít [Cloud Shell](../cloud-shell/overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-135">You can use [Cloud Shell](../cloud-shell/overview.md) to run the Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="6a250-136">Nejprve je však nutné šablonu nahrát do sdílené složky pro vaši službu Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6a250-136">However, you must first load your template into the file share for your Cloud Shell.</span></span> <span data-ttu-id="6a250-137">Pokud jste ještě službu Cloud Shell nepoužívali, přečtěte si téma [Přehled služby Azure Cloud Shell](../cloud-shell/overview.md), kde najdete informace o jejím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6a250-137">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="6a250-138">Přihlaste se k [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6a250-138">Log in to the [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="6a250-139">Vyberte vaši skupinu prostředků služby Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6a250-139">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="6a250-140">Vzor názvů je `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="6a250-140">The name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Výběr skupiny prostředků](./media/resource-group-template-deploy-cli/select-cs-resource-group.png)

3. <span data-ttu-id="6a250-142">Vyberte účet úložiště pro službu Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6a250-142">Select the storage account for your Cloud Shell.</span></span>

   ![Výběr účtu úložiště](./media/resource-group-template-deploy-cli/select-storage.png)

4. <span data-ttu-id="6a250-144">Vyberte **Soubory**.</span><span class="sxs-lookup"><span data-stu-id="6a250-144">Select **Files**.</span></span>

   ![Výběr souborů](./media/resource-group-template-deploy-cli/select-files.png)

5. <span data-ttu-id="6a250-146">Vyberte sdílenou složku pro službu Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6a250-146">Select the file share for Cloud Shell.</span></span> <span data-ttu-id="6a250-147">Vzor názvů je `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="6a250-147">The name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Výběr sdílené složky](./media/resource-group-template-deploy-cli/select-file-share.png)

6. <span data-ttu-id="6a250-149">Vyberte **Přidat adresář**.</span><span class="sxs-lookup"><span data-stu-id="6a250-149">Select **Add directory**.</span></span>

   ![Přidání adresáře](./media/resource-group-template-deploy-cli/select-add-directory.png)

7. <span data-ttu-id="6a250-151">Pojmenujte ho **templates** a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a250-151">Name it **templates**, and select **Okay**.</span></span>

   ![Pojmenování adresáře](./media/resource-group-template-deploy-cli/name-templates.png)

8. <span data-ttu-id="6a250-153">Vyberte nový adresář.</span><span class="sxs-lookup"><span data-stu-id="6a250-153">Select your new directory.</span></span>

   ![Výběr adresáře](./media/resource-group-template-deploy-cli/select-templates.png)

9. <span data-ttu-id="6a250-155">Vyberte **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="6a250-155">Select **Upload**.</span></span>

   ![Výběr nahrání](./media/resource-group-template-deploy-cli/select-upload.png)

10. <span data-ttu-id="6a250-157">Vyhledejte a nahrajte vaši šablonu.</span><span class="sxs-lookup"><span data-stu-id="6a250-157">Find and upload your template.</span></span>

   ![Nahrání souboru](./media/resource-group-template-deploy-cli/upload-files.png)

11. <span data-ttu-id="6a250-159">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="6a250-159">Open the prompt.</span></span>

   ![Otevření služby Cloud Shell](./media/resource-group-template-deploy-cli/start-cloud-shell.png)

12. <span data-ttu-id="6a250-161">Ve službě Cloud Shell zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6a250-161">Enter the following commands in the Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a><span data-ttu-id="6a250-162">Soubory parametrů</span><span class="sxs-lookup"><span data-stu-id="6a250-162">Parameter files</span></span>

<span data-ttu-id="6a250-163">Místo předávání parametrů jako vložené hodnoty ve vašem skriptu, možná bude jednodušší použít soubor JSON, který obsahuje hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="6a250-163">Rather than passing parameters as inline values in your script, you may find it easier to use a JSON file that contains the parameter values.</span></span> <span data-ttu-id="6a250-164">Soubor parametrů musí být v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="6a250-164">The parameter file must be in the following format:</span></span>

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

<span data-ttu-id="6a250-165">Všimněte si, že sekci parametrů obsahuje název parametru, který odpovídá parametru definované v šabloně (storageAccountType).</span><span class="sxs-lookup"><span data-stu-id="6a250-165">Notice that the parameters section includes a parameter name that matches the parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="6a250-166">Soubor parametrů obsahuje hodnotu pro parametr.</span><span class="sxs-lookup"><span data-stu-id="6a250-166">The parameter file contains a value for the parameter.</span></span> <span data-ttu-id="6a250-167">Tato hodnota se automaticky předán do šablony během nasazení.</span><span class="sxs-lookup"><span data-stu-id="6a250-167">This value is automatically passed to the template during deployment.</span></span> <span data-ttu-id="6a250-168">Můžete vytvořit několik souborů parametr pro různé scénáře nasazení a pak předejte soubor odpovídající parametr.</span><span class="sxs-lookup"><span data-stu-id="6a250-168">You can create multiple parameter files for different deployment scenarios, and then pass in the appropriate parameter file.</span></span> 

<span data-ttu-id="6a250-169">V předchozím příkladu zkopírujte a uložte ho jako soubor s názvem `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="6a250-169">Copy the preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="6a250-170">Chcete-li předat soubor místní parametrů, použijte `@` zadat místní soubor s názvem storage.parameters.json.</span><span class="sxs-lookup"><span data-stu-id="6a250-170">To pass a local parameter file, use `@` to specify a local file named storage.parameters.json.</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a><span data-ttu-id="6a250-171">Testovací nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="6a250-171">Test a template deployment</span></span>

<span data-ttu-id="6a250-172">K otestování šablony a parametr hodnoty bez ve skutečnosti nasazení všechny prostředky, použijte [ověření nasazení skupiny az](/cli/azure/group/deployment#validate).</span><span class="sxs-lookup"><span data-stu-id="6a250-172">To test your template and parameter values without actually deploying any resources, use [az group deployment validate](/cli/azure/group/deployment#validate).</span></span> 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

<span data-ttu-id="6a250-173">Pokud nejsou zjištěny žádné chyby, příkaz vrátí informace o testovací nasazení.</span><span class="sxs-lookup"><span data-stu-id="6a250-173">If no errors are detected, the command returns information about the test deployment.</span></span> <span data-ttu-id="6a250-174">Konkrétně, Všimněte si, že **chyba** hodnota je null.</span><span class="sxs-lookup"><span data-stu-id="6a250-174">In particular, notice that the **error** value is null.</span></span>

```azurecli
{
  "error": null,
  "properties": {
      ...
```

<span data-ttu-id="6a250-175">Pokud dojde k chybě, příkaz vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="6a250-175">If an error is detected, the command returns an error message.</span></span> <span data-ttu-id="6a250-176">Probíhá pokus o předání nesprávná hodnota pro účet úložiště SKU, například vrátí následující chybu:</span><span class="sxs-lookup"><span data-stu-id="6a250-176">For example, attempting to pass an incorrect value for the storage account SKU, returns the following error:</span></span>

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

<span data-ttu-id="6a250-177">Pokud vaše šablona obsahuje chybu syntaxe, příkaz vrátí chybu oznamující, že ho nebylo možné rozložit šablony.</span><span class="sxs-lookup"><span data-stu-id="6a250-177">If your template has a syntax error, the command returns an error indicating it could not parse the template.</span></span> <span data-ttu-id="6a250-178">Zpráva označuje číslo řádku a pozice chybu analýzy.</span><span class="sxs-lookup"><span data-stu-id="6a250-178">The message indicates the line number and position of the parsing error.</span></span>

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

<span data-ttu-id="6a250-179">Pokud chcete použít dokončení režimu, použijte `mode` parametr:</span><span class="sxs-lookup"><span data-stu-id="6a250-179">To use complete mode, use the `mode` parameter:</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a><span data-ttu-id="6a250-180">Ukázka šablony</span><span class="sxs-lookup"><span data-stu-id="6a250-180">Sample template</span></span>

<span data-ttu-id="6a250-181">Příklady v tomto tématu slouží následující šablony.</span><span class="sxs-lookup"><span data-stu-id="6a250-181">The following template is used for the examples in this topic.</span></span> <span data-ttu-id="6a250-182">Zkopírujte a uložte ho jako soubor s názvem storage.json.</span><span class="sxs-lookup"><span data-stu-id="6a250-182">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="6a250-183">Chcete-li pochopit, jak k vytvoření této šablony, přečtěte si téma [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-183">To understand how to create this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="6a250-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a250-184">Next steps</span></span>
* <span data-ttu-id="6a250-185">V příkladech v tomto článku nasadit do skupiny prostředků v rámci vašeho předplatného výchozí prostředky.</span><span class="sxs-lookup"><span data-stu-id="6a250-185">The examples in this article deploy resources to a resource group in your default subscription.</span></span> <span data-ttu-id="6a250-186">Použijte jiný odběr, najdete v tématu [spravovat víc předplatných Azure](/cli/azure/manage-azure-subscriptions-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6a250-186">To use a different subscription, see [Manage multiple Azure subscriptions](/cli/azure/manage-azure-subscriptions-azure-cli).</span></span>
* <span data-ttu-id="6a250-187">Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript nasazení šablony Resource Manageru](resource-manager-samples-cli-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-187">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-cli-deploy.md).</span></span>
* <span data-ttu-id="6a250-188">Chcete-li pochopit, jak definovat parametry v šabloně, přečtěte si téma [pochopit strukturu a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-188">To understand how to define parameters in your template, see [Understand the structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="6a250-189">Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-189">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="6a250-190">Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-190">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>
* <span data-ttu-id="6a250-191">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6a250-191">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
