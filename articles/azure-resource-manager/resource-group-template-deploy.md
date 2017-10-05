---
title: "Nasazení prostředků pomocí prostředí PowerShell a šablony | Microsoft Docs"
description: "Nasazení prostředky do Azure pomocí Azure Resource Manageru a prostředí Azure PowerShell. Prostředky jsou definovány v šabloně Resource Manageru."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 5f395abf8ebdfbac18fd17d8183b392673e280ec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a><span data-ttu-id="2be8d-104">Nasazení prostředků pomocí šablon Resource Manageru a Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="2be8d-104">Deploy resources with Resource Manager templates and Azure PowerShell</span></span>

<span data-ttu-id="2be8d-105">Toto téma vysvětluje, jak pomocí prostředí Azure PowerShell s Resource Manager šablony nasazení vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="2be8d-105">This topic explains how to use Azure PowerShell with Resource Manager templates to deploy your resources to Azure.</span></span> <span data-ttu-id="2be8d-106">Pokud nejste obeznámeni s koncepty nasazení a Správa řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2be8d-106">If you are not familiar with the concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

<span data-ttu-id="2be8d-107">Šablony Resource Manageru, který nasazujete, může to být místní soubor na počítači, nebo externí soubor, který je umístěný v úložišti, jako je Githubu.</span><span class="sxs-lookup"><span data-stu-id="2be8d-107">The Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="2be8d-108">Šablona nasazení v tomto článku je k dispozici v [Ukázka šablony](#sample-template) oddílu, nebo jako [Šablona účtu úložiště na webu GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="2be8d-108">The template you deploy in this article is available in the [Sample template](#sample-template) section, or as [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a><span data-ttu-id="2be8d-109">Nasazení šablony z místního počítače</span><span class="sxs-lookup"><span data-stu-id="2be8d-109">Deploy a template from your local machine</span></span>

<span data-ttu-id="2be8d-110">Při nasazování prostředků do Azure, můžete:</span><span class="sxs-lookup"><span data-stu-id="2be8d-110">When deploying resources to Azure, you:</span></span>

1. <span data-ttu-id="2be8d-111">Přihlaste se k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="2be8d-111">Log in to your Azure account</span></span>
2. <span data-ttu-id="2be8d-112">Vytvořte skupinu prostředků, která slouží jako kontejner pro nasazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="2be8d-112">Create a resource group that serves as the container for the deployed resources.</span></span> <span data-ttu-id="2be8d-113">Název skupiny prostředků může obsahovat pouze alfanumerické znaky, tečky, podtržítka, pomlčky a závorky.</span><span class="sxs-lookup"><span data-stu-id="2be8d-113">The name of the resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="2be8d-114">Může být až 90 znaků.</span><span class="sxs-lookup"><span data-stu-id="2be8d-114">It can be up to 90 characters.</span></span> <span data-ttu-id="2be8d-115">Nemůže končit tečkou.</span><span class="sxs-lookup"><span data-stu-id="2be8d-115">It cannot end in a period.</span></span>
3. <span data-ttu-id="2be8d-116">Nasazení do skupiny prostředků definující zdrojů pro vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="2be8d-116">Deploy to the resource group the template that defines the resources to create</span></span>

<span data-ttu-id="2be8d-117">Šablonu může obsahovat parametry, které vám umožní přizpůsobit nasazení.</span><span class="sxs-lookup"><span data-stu-id="2be8d-117">A template can include parameters that enable you to customize the deployment.</span></span> <span data-ttu-id="2be8d-118">Například můžete zadat hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním).</span><span class="sxs-lookup"><span data-stu-id="2be8d-118">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="2be8d-119">Ukázka šablony definuje parametr pro účet úložiště SKU.</span><span class="sxs-lookup"><span data-stu-id="2be8d-119">The sample template defines a parameter for the storage account SKU.</span></span>

<span data-ttu-id="2be8d-120">Následující příklad vytvoří skupinu prostředků a nasadí šablonu z místního počítače:</span><span class="sxs-lookup"><span data-stu-id="2be8d-120">The following example creates a resource group, and deploys a template from your local machine:</span></span>

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="2be8d-121">Dokončení nasazení může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="2be8d-121">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="2be8d-122">Po dokončení zobrazí zprávu, která obsahuje výsledek:</span><span class="sxs-lookup"><span data-stu-id="2be8d-122">When it finishes, you see a message that includes the result:</span></span>

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a><span data-ttu-id="2be8d-123">Nasazení šablony z externího zdroje</span><span class="sxs-lookup"><span data-stu-id="2be8d-123">Deploy a template from an external source</span></span>

<span data-ttu-id="2be8d-124">Místo uložení šablony Resource Manageru na místním počítači, dáváte přednost uložit je do externího umístění.</span><span class="sxs-lookup"><span data-stu-id="2be8d-124">Instead of storing Resource Manager templates on your local machine, you may prefer to store them in an external location.</span></span> <span data-ttu-id="2be8d-125">Šablony můžete uložit ve úložiště řízení zdrojů (například Githubu).</span><span class="sxs-lookup"><span data-stu-id="2be8d-125">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="2be8d-126">Nebo byste je uložit v účtu úložiště Azure pro přístup ke sdílenému ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="2be8d-126">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="2be8d-127">Chcete-li nasadit externí šablonu, použijte **TemplateUri** parametr.</span><span class="sxs-lookup"><span data-stu-id="2be8d-127">To deploy an external template, use the **TemplateUri** parameter.</span></span> <span data-ttu-id="2be8d-128">Identifikátor URI v příkladu použijte k nasazení ukázkové šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="2be8d-128">Use the URI in the example to deploy the sample template from GitHub.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

<span data-ttu-id="2be8d-129">V předchozím příkladu vyžaduje veřejně přístupná identifikátor URI pro šablony, která funguje pro většinu scénářů, protože vaše šablona by neměla zahrnovat citlivá data.</span><span class="sxs-lookup"><span data-stu-id="2be8d-129">The preceding example requires a publicly accessible URI for the template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="2be8d-130">Pokud budete muset zadat citlivá data (např. heslo správce), předejte jako parametr zabezpečené tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2be8d-130">If you need to specify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="2be8d-131">Ale pokud nechcete, aby vaše šablona veřejně přístupný, můžete chránit jeho uložením v kontejneru privátní úložiště.</span><span class="sxs-lookup"><span data-stu-id="2be8d-131">However, if you do not want your template to be publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="2be8d-132">Informace o nasazení šablony, která vyžaduje token sdílený přístupový podpis (SAS) najdete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="2be8d-132">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>

## <a name="parameter-files"></a><span data-ttu-id="2be8d-133">Soubory parametrů</span><span class="sxs-lookup"><span data-stu-id="2be8d-133">Parameter files</span></span>

<span data-ttu-id="2be8d-134">Místo předávání parametrů jako vložené hodnoty ve vašem skriptu, možná bude jednodušší použít soubor JSON, který obsahuje hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="2be8d-134">Rather than passing parameters as inline values in your script, you may find it easier to use a JSON file that contains the parameter values.</span></span> <span data-ttu-id="2be8d-135">Soubor parametrů musí být v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="2be8d-135">The parameter file must be in the following format:</span></span>

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

<span data-ttu-id="2be8d-136">Všimněte si, že sekci parametrů obsahuje název parametru, který odpovídá parametru definované v šabloně (storageAccountType).</span><span class="sxs-lookup"><span data-stu-id="2be8d-136">Notice that the parameters section includes a parameter name that matches the parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="2be8d-137">Soubor parametrů obsahuje hodnotu pro parametr.</span><span class="sxs-lookup"><span data-stu-id="2be8d-137">The parameter file contains a value for the parameter.</span></span> <span data-ttu-id="2be8d-138">Tato hodnota se automaticky předán do šablony během nasazení.</span><span class="sxs-lookup"><span data-stu-id="2be8d-138">This value is automatically passed to the template during deployment.</span></span> <span data-ttu-id="2be8d-139">Můžete vytvořit několik souborů parametr pro různé scénáře nasazení a pak předejte soubor odpovídající parametr.</span><span class="sxs-lookup"><span data-stu-id="2be8d-139">You can create multiple parameter files for different deployment scenarios, and then pass in the appropriate parameter file.</span></span> 

<span data-ttu-id="2be8d-140">V předchozím příkladu zkopírujte a uložte ho jako soubor s názvem `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="2be8d-140">Copy the preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="2be8d-141">Chcete-li předat soubor místní parametrů, použijte **TemplateParameterFile** parametr:</span><span class="sxs-lookup"><span data-stu-id="2be8d-141">To pass a local parameter file, use the **TemplateParameterFile** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

<span data-ttu-id="2be8d-142">Chcete-li předat soubor externí parametr, použijte **TemplateParameterUri** parametr:</span><span class="sxs-lookup"><span data-stu-id="2be8d-142">To pass an external parameter file, use the **TemplateParameterUri** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

<span data-ttu-id="2be8d-143">Můžete použít vložené parametry a soubor místní parametru v rámci jedné operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="2be8d-143">You can use inline parameters and a local parameter file in the same deployment operation.</span></span> <span data-ttu-id="2be8d-144">Můžete například zadat některé hodnoty v souboru místní parametr a přidání dalších hodnot vloženého během nasazení.</span><span class="sxs-lookup"><span data-stu-id="2be8d-144">For example, you can specify some values in the local parameter file and add other values inline during deployment.</span></span> <span data-ttu-id="2be8d-145">Když poskytujete hodnoty pro parametr v parametru místního souboru a vložené, hodnota vložené přednost.</span><span class="sxs-lookup"><span data-stu-id="2be8d-145">If you provide values for a parameter in both the local parameter file and inline, the inline value takes precedence.</span></span>

<span data-ttu-id="2be8d-146">Ale při použití souboru externí parametr nemůžete předat další hodnoty buď vložené nebo z místního souboru.</span><span class="sxs-lookup"><span data-stu-id="2be8d-146">However, when you use an external parameter file, you cannot pass other values either inline or from a local file.</span></span> <span data-ttu-id="2be8d-147">Pokud zadáte soubor parametrů v **TemplateParameterUri** parametr, všechny vnořené parametry jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="2be8d-147">When you specify a parameter file in the **TemplateParameterUri** parameter, all inline parameters are ignored.</span></span> <span data-ttu-id="2be8d-148">Zadejte všechny hodnoty parametrů v externím souboru.</span><span class="sxs-lookup"><span data-stu-id="2be8d-148">Provide all parameter values in the external file.</span></span> <span data-ttu-id="2be8d-149">Pokud vaše šablona obsahuje citlivé hodnotu, která nelze zahrnout do souboru parametrů, přidejte tuto hodnotu do trezoru klíčů, nebo dynamicky poskytovat všechny vnořené hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="2be8d-149">If your template includes a sensitive value that you cannot include in the parameter file, either add that value to a key vault, or dynamically provide all parameter values inline.</span></span>

<span data-ttu-id="2be8d-150">Pokud vaše šablona obsahuje parametr se stejným názvem jako jeden z parametrů v příkazu prostředí PowerShell, prostředí PowerShell nabídne parametru z šablony operátory **FromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="2be8d-150">If your template includes a parameter with the same name as one of the parameters in the PowerShell command, PowerShell presents the parameter from your template with the postfix **FromTemplate**.</span></span> <span data-ttu-id="2be8d-151">Například parametr s názvem **ResourceGroupName** na šablony v konfliktu s **ResourceGroupName** parametr ve [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) rutiny.</span><span class="sxs-lookup"><span data-stu-id="2be8d-151">For example, a parameter named **ResourceGroupName** in your template conflicts with the **ResourceGroupName** parameter in the [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="2be8d-152">Zobrazí se výzva k zadání hodnotu **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="2be8d-152">You are prompted to provide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="2be8d-153">Obecně platí neměli byste toto nedorozuměním není pojmenováním parametry se stejným názvem jako parametry použité pro operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="2be8d-153">In general, you should avoid this confusion by not naming parameters with the same name as parameters used for deployment operations.</span></span>

## <a name="test-a-template-deployment"></a><span data-ttu-id="2be8d-154">Testovací nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="2be8d-154">Test a template deployment</span></span>

<span data-ttu-id="2be8d-155">K otestování šablony a parametr hodnoty bez ve skutečnosti nasazení všechny prostředky, použijte [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span><span class="sxs-lookup"><span data-stu-id="2be8d-155">To test your template and parameter values without actually deploying any resources, use [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span></span> 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="2be8d-156">Pokud nejsou zjištěny žádné chyby, dokončení příkazu bez odezvy.</span><span class="sxs-lookup"><span data-stu-id="2be8d-156">If no errors are detected, the command finishes without a response.</span></span> <span data-ttu-id="2be8d-157">Pokud dojde k chybě, příkaz vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="2be8d-157">If an error is detected, the command returns an error message.</span></span> <span data-ttu-id="2be8d-158">Probíhá pokus o předání nesprávná hodnota pro účet úložiště SKU, například vrátí následující chybu:</span><span class="sxs-lookup"><span data-stu-id="2be8d-158">For example, attempting to pass an incorrect value for the storage account SKU, returns the following error:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

<span data-ttu-id="2be8d-159">Pokud vaše šablona obsahuje chybu syntaxe, příkaz vrátí chybu oznamující, že ho nebylo možné rozložit šablony.</span><span class="sxs-lookup"><span data-stu-id="2be8d-159">If your template has a syntax error, the command returns an error indicating it could not parse the template.</span></span> <span data-ttu-id="2be8d-160">Zpráva označuje číslo řádku a pozice chybu analýzy.</span><span class="sxs-lookup"><span data-stu-id="2be8d-160">The message indicates the line number and position of the parsing error.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="2be8d-161">Pokud chcete použít dokončení režimu, použijte `Mode` parametr:</span><span class="sxs-lookup"><span data-stu-id="2be8d-161">To use complete mode, use the `Mode` parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a><span data-ttu-id="2be8d-162">Ukázka šablony</span><span class="sxs-lookup"><span data-stu-id="2be8d-162">Sample template</span></span>

<span data-ttu-id="2be8d-163">Příklady v tomto tématu slouží následující šablony.</span><span class="sxs-lookup"><span data-stu-id="2be8d-163">The following template is used for the examples in this topic.</span></span> <span data-ttu-id="2be8d-164">Zkopírujte a uložte ho jako soubor s názvem storage.json.</span><span class="sxs-lookup"><span data-stu-id="2be8d-164">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="2be8d-165">Chcete-li pochopit, jak k vytvoření této šablony, přečtěte si téma [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="2be8d-165">To understand how to create this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="2be8d-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2be8d-166">Next steps</span></span>
* <span data-ttu-id="2be8d-167">V příkladech v tomto článku nasadit do skupiny prostředků v rámci vašeho předplatného výchozí prostředky.</span><span class="sxs-lookup"><span data-stu-id="2be8d-167">The examples in this article deploy resources to a resource group in your default subscription.</span></span> <span data-ttu-id="2be8d-168">Použijte jiný odběr, najdete v tématu [spravovat víc předplatných Azure](/powershell/azure/manage-subscriptions-azureps).</span><span class="sxs-lookup"><span data-stu-id="2be8d-168">To use a different subscription, see [Manage multiple Azure subscriptions](/powershell/azure/manage-subscriptions-azureps).</span></span>
* <span data-ttu-id="2be8d-169">Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript nasazení šablony Resource Manageru](resource-manager-samples-powershell-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="2be8d-169">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-powershell-deploy.md).</span></span>
* <span data-ttu-id="2be8d-170">Chcete-li pochopit, jak definovat parametry v šabloně, přečtěte si téma [pochopit strukturu a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2be8d-170">To understand how to define parameters in your template, see [Understand the structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="2be8d-171">Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="2be8d-171">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="2be8d-172">Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="2be8d-172">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="2be8d-173">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="2be8d-173">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

