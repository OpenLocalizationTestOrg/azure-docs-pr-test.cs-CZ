---
title: "aaaDeploy prostředků pomocí prostředí PowerShell a šablony | Microsoft Docs"
description: "Pomocí Azure Resource Manageru a prostředí Azure PowerShell toodeploy tooAzure prostředky. Hello prostředky jsou definovány v šabloně Resource Manager."
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
ms.openlocfilehash: 41506811ba3c2ea5df6313db70978ade50f71161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a><span data-ttu-id="1b0ba-104">Nasazení prostředků pomocí šablon Resource Manageru a Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="1b0ba-104">Deploy resources with Resource Manager templates and Azure PowerShell</span></span>

<span data-ttu-id="1b0ba-105">Toto téma vysvětluje, jak toouse prostředí Azure PowerShell s Resource Manager šablony toodeploy tooAzure vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-105">This topic explains how toouse Azure PowerShell with Resource Manager templates toodeploy your resources tooAzure.</span></span> <span data-ttu-id="1b0ba-106">Pokud nejste obeznámeni s hello koncepty nasazení a Správa řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-106">If you are not familiar with hello concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

<span data-ttu-id="1b0ba-107">šablony Resource Manageru Hello nasadíte, může to být místní soubor na počítači, nebo externí soubor, který je umístěný v úložišti, jako je Githubu.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-107">hello Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="1b0ba-108">Šablona Hello nasazení v tomto článku je k dispozici v hello [vzorové šablony](#sample-template) oddílu, nebo jako [Šablona účtu úložiště na webu GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-108">hello template you deploy in this article is available in hello [Sample template](#sample-template) section, or as [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a><span data-ttu-id="1b0ba-109">Nasazení šablony z místního počítače</span><span class="sxs-lookup"><span data-stu-id="1b0ba-109">Deploy a template from your local machine</span></span>

<span data-ttu-id="1b0ba-110">Při nasazování tooAzure prostředků, můžete:</span><span class="sxs-lookup"><span data-stu-id="1b0ba-110">When deploying resources tooAzure, you:</span></span>

1. <span data-ttu-id="1b0ba-111">Přihlaste se tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="1b0ba-111">Log in tooyour Azure account</span></span>
2. <span data-ttu-id="1b0ba-112">Vytvořte skupinu prostředků, která slouží jako kontejner hello hello nasazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-112">Create a resource group that serves as hello container for hello deployed resources.</span></span> <span data-ttu-id="1b0ba-113">Hello název hello skupiny prostředků může obsahovat jenom alfanumerické znaky, tečky, podtržítka, pomlčky a závorky.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-113">hello name of hello resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="1b0ba-114">Může být až too90 znaků.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-114">It can be up too90 characters.</span></span> <span data-ttu-id="1b0ba-115">Nemůže končit tečkou.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-115">It cannot end in a period.</span></span>
3. <span data-ttu-id="1b0ba-116">Nasazení toohello prostředků skupiny hello šablony, který definuje prostředky toocreate hello</span><span class="sxs-lookup"><span data-stu-id="1b0ba-116">Deploy toohello resource group hello template that defines hello resources toocreate</span></span>

<span data-ttu-id="1b0ba-117">Šablona může obsahovat parametry, které umožňují toocustomize hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-117">A template can include parameters that enable you toocustomize hello deployment.</span></span> <span data-ttu-id="1b0ba-118">Například můžete zadat hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-118">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="1b0ba-119">Ukázka šablony Hello definuje parametr pro účet úložiště hello SKU.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-119">hello sample template defines a parameter for hello storage account SKU.</span></span>

<span data-ttu-id="1b0ba-120">Hello následující ukázka vytvoří skupinu prostředků a nasadí šablonu z místního počítače:</span><span class="sxs-lookup"><span data-stu-id="1b0ba-120">hello following example creates a resource group, and deploys a template from your local machine:</span></span>

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="1b0ba-121">Hello nasazení může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-121">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="1b0ba-122">Po dokončení zobrazí zprávu, která zahrnuje hello výsledek:</span><span class="sxs-lookup"><span data-stu-id="1b0ba-122">When it finishes, you see a message that includes hello result:</span></span>

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a><span data-ttu-id="1b0ba-123">Nasazení šablony z externího zdroje</span><span class="sxs-lookup"><span data-stu-id="1b0ba-123">Deploy a template from an external source</span></span>

<span data-ttu-id="1b0ba-124">Místo uložení šablony Resource Manageru na místním počítači, dáte možná přednost toostore je v externí umístění.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-124">Instead of storing Resource Manager templates on your local machine, you may prefer toostore them in an external location.</span></span> <span data-ttu-id="1b0ba-125">Šablony můžete uložit ve úložiště řízení zdrojů (například Githubu).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-125">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="1b0ba-126">Nebo byste je uložit v účtu úložiště Azure pro přístup ke sdílenému ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-126">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="1b0ba-127">toodeploy šablonu externí použít hello **TemplateUri** parametr.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-127">toodeploy an external template, use hello **TemplateUri** parameter.</span></span> <span data-ttu-id="1b0ba-128">Použijte hello URI v hello příklad toodeploy hello Ukázka šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-128">Use hello URI in hello example toodeploy hello sample template from GitHub.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

<span data-ttu-id="1b0ba-129">Hello předchozí příklad vyžaduje veřejně přístupná identifikátor URI pro hello šablony, která funguje pro většinu scénářů, protože vaše šablona by neměla zahrnovat citlivá data.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-129">hello preceding example requires a publicly accessible URI for hello template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="1b0ba-130">Pokud potřebujete toospecify citlivá data (např. heslo správce), předejte jako parametr zabezpečené tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-130">If you need toospecify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="1b0ba-131">Ale pokud nechcete, aby vaše šablona toobe veřejně přístupný, můžete chránit ho ukládáním do kontejner privátní úložiště.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-131">However, if you do not want your template toobe publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="1b0ba-132">Informace o nasazení šablony, která vyžaduje token sdílený přístupový podpis (SAS) najdete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-132">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>

## <a name="parameter-files"></a><span data-ttu-id="1b0ba-133">Soubory parametrů</span><span class="sxs-lookup"><span data-stu-id="1b0ba-133">Parameter files</span></span>

<span data-ttu-id="1b0ba-134">Místo předávání parametrů jako vložené hodnoty ve vašem skriptu, možná bude snazší toouse soubor JSON, který obsahuje hodnoty parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-134">Rather than passing parameters as inline values in your script, you may find it easier toouse a JSON file that contains hello parameter values.</span></span> <span data-ttu-id="1b0ba-135">soubor parametrů Hello musí být ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="1b0ba-135">hello parameter file must be in hello following format:</span></span>

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

<span data-ttu-id="1b0ba-136">Všimněte si, že část parametry hello obsahuje název parametru, který odpovídá hello parametr definovaný v šabloně (storageAccountType).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-136">Notice that hello parameters section includes a parameter name that matches hello parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="1b0ba-137">Hello soubor parametrů obsahuje hodnotu pro parametr hello.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-137">hello parameter file contains a value for hello parameter.</span></span> <span data-ttu-id="1b0ba-138">Tato hodnota je předán toohello šablony automaticky během nasazení.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-138">This value is automatically passed toohello template during deployment.</span></span> <span data-ttu-id="1b0ba-139">Můžete vytvořit několik souborů parametr pro různé scénáře nasazení a pak předejte soubor hello odpovídající parametr.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-139">You can create multiple parameter files for different deployment scenarios, and then pass in hello appropriate parameter file.</span></span> 

<span data-ttu-id="1b0ba-140">Zkopírujte hello předcházející příklad a uložte ho jako soubor s názvem `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-140">Copy hello preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="1b0ba-141">toopass soubor místní parametr použít hello **TemplateParameterFile** parametr:</span><span class="sxs-lookup"><span data-stu-id="1b0ba-141">toopass a local parameter file, use hello **TemplateParameterFile** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

<span data-ttu-id="1b0ba-142">toopass soubor externí parametr, použijte hello **TemplateParameterUri** parametr:</span><span class="sxs-lookup"><span data-stu-id="1b0ba-142">toopass an external parameter file, use hello **TemplateParameterUri** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

<span data-ttu-id="1b0ba-143">Můžete použít vložené parametry a místní parametr souborů v hello stejné operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-143">You can use inline parameters and a local parameter file in hello same deployment operation.</span></span> <span data-ttu-id="1b0ba-144">Můžete například zadat některé hodnoty v souboru místní parametr hello a přidání dalších hodnot vloženého během nasazení.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-144">For example, you can specify some values in hello local parameter file and add other values inline during deployment.</span></span> <span data-ttu-id="1b0ba-145">Když poskytujete hodnoty pro parametr v souboru místní parametr hello i vložené, hello vložené přednost hodnota.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-145">If you provide values for a parameter in both hello local parameter file and inline, hello inline value takes precedence.</span></span>

<span data-ttu-id="1b0ba-146">Ale při použití souboru externí parametr nemůžete předat další hodnoty buď vložené nebo z místního souboru.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-146">However, when you use an external parameter file, you cannot pass other values either inline or from a local file.</span></span> <span data-ttu-id="1b0ba-147">Pokud zadáte soubor parametrů v hello **TemplateParameterUri** parametr, všechny vnořené parametry jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-147">When you specify a parameter file in hello **TemplateParameterUri** parameter, all inline parameters are ignored.</span></span> <span data-ttu-id="1b0ba-148">Zadejte všechny hodnoty parametrů v souboru externích hello.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-148">Provide all parameter values in hello external file.</span></span> <span data-ttu-id="1b0ba-149">Pokud vaše šablona obsahuje citlivé hodnotu, která nelze zahrnout do souboru parametrů hello, přidejte tuto hodnotu trezoru klíčů tooa nebo dynamicky poskytovat všechny vnořené hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-149">If your template includes a sensitive value that you cannot include in hello parameter file, either add that value tooa key vault, or dynamically provide all parameter values inline.</span></span>

<span data-ttu-id="1b0ba-150">Pokud vaše šablona obsahuje parametr s hello stejný název jako jeden z parametrů hello v hello příkaz prostředí PowerShell, prostředí PowerShell představuje parametr hello z šablony s operátory hello **FromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-150">If your template includes a parameter with hello same name as one of hello parameters in hello PowerShell command, PowerShell presents hello parameter from your template with hello postfix **FromTemplate**.</span></span> <span data-ttu-id="1b0ba-151">Například parametr s názvem **ResourceGroupName** na šablony v konfliktu s hello **ResourceGroupName** parametr v hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)rutiny.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-151">For example, a parameter named **ResourceGroupName** in your template conflicts with hello **ResourceGroupName** parameter in hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="1b0ba-152">Jsou výzvami tooprovide hodnotu **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-152">You are prompted tooprovide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="1b0ba-153">Obecně platí neměli byste toto nedorozuměním není pojmenováním parametry s hello stejný název jako parametry použité pro operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-153">In general, you should avoid this confusion by not naming parameters with hello same name as parameters used for deployment operations.</span></span>

## <a name="test-a-template-deployment"></a><span data-ttu-id="1b0ba-154">Testovací nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="1b0ba-154">Test a template deployment</span></span>

<span data-ttu-id="1b0ba-155">pomocí šablony a parametr hodnoty bez ve skutečnosti nasazení všechny prostředky tootest [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-155">tootest your template and parameter values without actually deploying any resources, use [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span></span> 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="1b0ba-156">Pokud nejsou zjištěny žádné chyby, dokončení příkazu hello bez odezvy.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-156">If no errors are detected, hello command finishes without a response.</span></span> <span data-ttu-id="1b0ba-157">Pokud dojde k chybě, hello příkaz vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-157">If an error is detected, hello command returns an error message.</span></span> <span data-ttu-id="1b0ba-158">Například pokus toopass nesprávná hodnota pro účet úložiště hello SKU, vrátí hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="1b0ba-158">For example, attempting toopass an incorrect value for hello storage account SKU, returns hello following error:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'hello provided value 'badSku' for hello template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. hello parameter value is not part of hello allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

<span data-ttu-id="1b0ba-159">Pokud vaše šablona obsahuje chybu syntaxe, příkaz hello chybovou zprávu oznamující, že ho nebylo možné rozložit hello šablony.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-159">If your template has a syntax error, hello command returns an error indicating it could not parse hello template.</span></span> <span data-ttu-id="1b0ba-160">uvítací zprávu označuje číslo řádku hello a pozice hello při analýze.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-160">hello message indicates hello line number and position of hello parsing error.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="1b0ba-161">režim dokončení toouse, použijte hello `Mode` parametr:</span><span class="sxs-lookup"><span data-stu-id="1b0ba-161">toouse complete mode, use hello `Mode` parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a><span data-ttu-id="1b0ba-162">Ukázka šablony</span><span class="sxs-lookup"><span data-stu-id="1b0ba-162">Sample template</span></span>

<span data-ttu-id="1b0ba-163">Hello následující šablona se používá pro hello příklady v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-163">hello following template is used for hello examples in this topic.</span></span> <span data-ttu-id="1b0ba-164">Zkopírujte a uložte ho jako soubor s názvem storage.json.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-164">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="1b0ba-165">toounderstand jak toocreate této šablony najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-165">toounderstand how toocreate this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="1b0ba-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b0ba-166">Next steps</span></span>
* <span data-ttu-id="1b0ba-167">Hello příklady v tomto článku nasazení skupiny prostředků tooa prostředky ve vašem předplatném výchozí.</span><span class="sxs-lookup"><span data-stu-id="1b0ba-167">hello examples in this article deploy resources tooa resource group in your default subscription.</span></span> <span data-ttu-id="1b0ba-168">toouse jiného předplatného, najdete v části [spravovat víc předplatných Azure](/powershell/azure/manage-subscriptions-azureps).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-168">toouse a different subscription, see [Manage multiple Azure subscriptions](/powershell/azure/manage-subscriptions-azureps).</span></span>
* <span data-ttu-id="1b0ba-169">Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript nasazení šablony Resource Manageru](resource-manager-samples-powershell-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-169">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-powershell-deploy.md).</span></span>
* <span data-ttu-id="1b0ba-170">jak zjistit, toodefine parametry v šabloně, toounderstand [pochopit strukturu hello a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-170">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="1b0ba-171">Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-171">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="1b0ba-172">Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-172">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="1b0ba-173">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="1b0ba-173">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

