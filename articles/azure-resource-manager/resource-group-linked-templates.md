---
title: "Odkaz šablony pro nasazení Azure | Microsoft Docs"
description: "Popisuje způsob použití propojených šablon v šablonu Azure Resource Manageru k vytvoření řešení modulární šablony. Ukazuje, jak chcete předat hodnoty parametrů, zadejte soubor parametrů a dynamicky vytvořené adresy URL."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 8b58a83ffd473500dd3f76c09e251f9208527d4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a><span data-ttu-id="631eb-104">Použití propojených šablon při nasazování prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="631eb-104">Using linked templates when deploying Azure resources</span></span>
<span data-ttu-id="631eb-105">Z v rámci jedné šablony Azure Resource Manager, můžete se propojit k jiné šablony, která umožňuje rozložit nasazení do sady s cílem, šablony pro konkrétní účel.</span><span class="sxs-lookup"><span data-stu-id="631eb-105">From within one Azure Resource Manager template, you can link to another template, which enables you to decompose your deployment into a set of targeted, purpose-specific templates.</span></span> <span data-ttu-id="631eb-106">Stejně jako u decomposing aplikace do více tříd kódu, poskytuje rozložením výhody z hlediska testování, opakované použití a přehlednosti.</span><span class="sxs-lookup"><span data-stu-id="631eb-106">As with decomposing an application into several code classes, decomposition provides benefits in terms of testing, reuse, and readability.</span></span>  

<span data-ttu-id="631eb-107">Z hlavní šablony lze předat parametry do propojené šablony, a tyto parametry můžete přímo namapovat parametry nebo proměnné vystavené volání šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-107">You can pass parameters from a main template to a linked template, and those parameters can directly map to parameters or variables exposed by the calling template.</span></span> <span data-ttu-id="631eb-108">Propojené šablony můžete předat také proměnnou výstup zpět na zdrojovou šablonu povolení výměna obousměrný dat mezi šablonami.</span><span class="sxs-lookup"><span data-stu-id="631eb-108">The linked template can also pass an output variable back to the source template, enabling a two-way data exchange between templates.</span></span>

## <a name="linking-to-a-template"></a><span data-ttu-id="631eb-109">Propojení do šablony</span><span class="sxs-lookup"><span data-stu-id="631eb-109">Linking to a template</span></span>
<span data-ttu-id="631eb-110">Můžete vytvořit propojení mezi přidáním nasazení prostředků v rámci hlavní šablony, která odkazuje na šabloně propojené dvě šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-110">You create a link between two templates by adding a deployment resource within the main template that points to the linked template.</span></span> <span data-ttu-id="631eb-111">Můžete nastavit **templateLink** vlastnost na identifikátor URI propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-111">You set the **templateLink** property to the URI of the linked template.</span></span> <span data-ttu-id="631eb-112">Zadáním hodnot parametru pro danou šablonu propojené přímo v šabloně nebo v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="631eb-112">You can provide parameter values for the linked template directly in your template or in a parameter file.</span></span> <span data-ttu-id="631eb-113">Následující příklad používá **parametry** vlastnost přímo zadat hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="631eb-113">The following example uses the **parameters** property to specify a parameter value directly.</span></span>

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

<span data-ttu-id="631eb-114">Podobně jako ostatní typy prostředků můžete nastavit závislosti mezi propojené šablony a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="631eb-114">Like other resource types, you can set dependencies between the linked template and other resources.</span></span> <span data-ttu-id="631eb-115">Proto pokud další prostředky vyžadují hodnotu výstup z propojené šablony, jste měli jistotu, že propojené šablony nasazení před sebou.</span><span class="sxs-lookup"><span data-stu-id="631eb-115">Therefore, when other resources require an output value from the linked template, you can make sure the linked template is deployed before them.</span></span> <span data-ttu-id="631eb-116">Nebo, pokud propojené šablony závisí na jiné prostředky, můžete zajistit, že jiné prostředky, které jsou nasazeny před propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-116">Or, when the linked template relies on other resources, you can make sure other resources are deployed before the linked template.</span></span> <span data-ttu-id="631eb-117">Načtení hodnoty z šablonu propojené s následující syntaxí:</span><span class="sxs-lookup"><span data-stu-id="631eb-117">You can retrieve a value from a linked template with the following syntax:</span></span>

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

<span data-ttu-id="631eb-118">Musí být mít přístup k šabloně propojené služby Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="631eb-118">The Resource Manager service must be able to access the linked template.</span></span> <span data-ttu-id="631eb-119">Nelze zadat místní soubor nebo soubor, který je k dispozici ve vaší místní síti propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-119">You cannot specify a local file or a file that is only available on your local network for the linked template.</span></span> <span data-ttu-id="631eb-120">Můžete pouze zadat hodnotu identifikátoru URI, která zahrnuje buď **http** nebo **https**.</span><span class="sxs-lookup"><span data-stu-id="631eb-120">You can only provide a URI value that includes either **http** or **https**.</span></span> <span data-ttu-id="631eb-121">Jednou z možností je umístit propojené šablony v účtu úložiště, a použijte identifikátor URI pro položku, tak jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="631eb-121">One option is to place your linked template in a storage account, and use the URI for that item, such as shown in the following example:</span></span>

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

<span data-ttu-id="631eb-122">I když propojené šablony musí být externě k dispozici, nemusí být obecně dostupné pro veřejnost.</span><span class="sxs-lookup"><span data-stu-id="631eb-122">Although the linked template must be externally available, it does not need to be generally available to the public.</span></span> <span data-ttu-id="631eb-123">Šablony můžete přidat na účet privátní úložiště, které je přístupné pouze majiteli účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="631eb-123">You can add your template to a private storage account that is accessible to only the storage account owner.</span></span> <span data-ttu-id="631eb-124">Pak vytvořte token sdílený přístupový podpis (SAS) pro povolení přístupu během nasazení.</span><span class="sxs-lookup"><span data-stu-id="631eb-124">Then, you create a shared access signature (SAS) token to enable access during deployment.</span></span> <span data-ttu-id="631eb-125">Přidejte tento token SAS URI propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-125">You add that SAS token to the URI for the linked template.</span></span> <span data-ttu-id="631eb-126">Postup nastavení šablony v účtu úložiště a generování tokenu SAS naleznete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md) nebo [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="631eb-126">For steps on setting up a template in a storage account and generating a SAS token, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md) or [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> 

<span data-ttu-id="631eb-127">Následující příklad ukazuje nadřazené šablony, který odkazuje na jinou šablonu.</span><span class="sxs-lookup"><span data-stu-id="631eb-127">The following example shows a parent template that links to another template.</span></span> <span data-ttu-id="631eb-128">Propojené šablony přistupuje s tokenem SAS, který se předává v jako parametr.</span><span class="sxs-lookup"><span data-stu-id="631eb-128">The linked template is accessed with a SAS token that is passed in as a parameter.</span></span>

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

<span data-ttu-id="631eb-129">I když token, je předaná jako zabezpečený řetězec, je identifikátor URI propojené šablony, včetně tokenu SAS, přihlášení operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="631eb-129">Even though the token is passed in as a secure string, the URI of the linked template, including the SAS token, is logged in the deployment operations.</span></span> <span data-ttu-id="631eb-130">K omezení rizika, nastavte vypršení platnosti pro daný token.</span><span class="sxs-lookup"><span data-stu-id="631eb-130">To limit exposure, set an expiration for the token.</span></span>

<span data-ttu-id="631eb-131">Správce prostředků zpracovává každé propojené šablony jako samostatné nasazení.</span><span class="sxs-lookup"><span data-stu-id="631eb-131">Resource Manager handles each linked template as a separate deployment.</span></span> <span data-ttu-id="631eb-132">V historii nasazení pro skupinu prostředků najdete v části samostatná nasazení pro nadřazené a vnořené šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-132">In the deployment history for the resource group, you see separate deployments for the parent and nested templates.</span></span>

![historie nasazení](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-to-a-parameter-file"></a><span data-ttu-id="631eb-134">Propojování do souboru parametrů</span><span class="sxs-lookup"><span data-stu-id="631eb-134">Linking to a parameter file</span></span>
<span data-ttu-id="631eb-135">Další příklad používá **parametersLink** vlastnost, která má odkaz na soubor parametru.</span><span class="sxs-lookup"><span data-stu-id="631eb-135">The next example uses the **parametersLink** property to link to a parameter file.</span></span>

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

<span data-ttu-id="631eb-136">Hodnota identifikátoru URI pro parametr propojené soubor nemůže být místní soubor a musí obsahovat buď **http** nebo **https**.</span><span class="sxs-lookup"><span data-stu-id="631eb-136">The URI value for the linked parameter file cannot be a local file, and must include either **http** or **https**.</span></span> <span data-ttu-id="631eb-137">Soubor parametrů také možné omezit přístup pomocí tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="631eb-137">The parameter file can also be limited to access through a SAS token.</span></span>

## <a name="using-variables-to-link-templates"></a><span data-ttu-id="631eb-138">Použití proměnných propojení šablony</span><span class="sxs-lookup"><span data-stu-id="631eb-138">Using variables to link templates</span></span>
<span data-ttu-id="631eb-139">Předchozí příklady nám ukázaly pevně definovaných hodnot adresu URL pro odkazy. šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-139">The previous examples showed hard-coded URL values for the template links.</span></span> <span data-ttu-id="631eb-140">Tento přístup může fungovat pro jednoduchou šablonu, ale nebude fungovat správně při práci s velké sady modulární šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-140">This approach might work for a simple template but it does not work well when working with a large set of modular templates.</span></span> <span data-ttu-id="631eb-141">Místo toho můžete vytvořit statickou proměnné, která ukládá základní adresu URL pro hlavní šablonu a dynamicky vytvářet adresy URL pro propojené šablony z této základní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="631eb-141">Instead, you can create a static variable that stores a base URL for the main template and then dynamically create URLs for the linked templates from that base URL.</span></span> <span data-ttu-id="631eb-142">Výhodou tohoto přístupu je snadno přesunout nebo rozvětvit šablony, protože potřebujete změnit statické proměnné v šabloně hlavní.</span><span class="sxs-lookup"><span data-stu-id="631eb-142">The benefit of this approach is you can easily move or fork the template because you only need to change the static variable in the main template.</span></span> <span data-ttu-id="631eb-143">Hlavní šablonu předá správné identifikátory URI v rámci rozložená šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-143">The main template passes the correct URIs throughout the decomposed template.</span></span>

<span data-ttu-id="631eb-144">Následující příklad ukazuje, jak použít základní adresu URL k vytvoření dvou adres URL pro propojených šablon (**sharedTemplateUrl** a **vmTemplate**).</span><span class="sxs-lookup"><span data-stu-id="631eb-144">The following example shows how to use a base URL to create two URLs for linked templates (**sharedTemplateUrl** and **vmTemplate**).</span></span> 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

<span data-ttu-id="631eb-145">Můžete také použít [deployment()](resource-group-template-functions-deployment.md#deployment) získat základní adresu URL pro aktuální šablony a použít k získání adresy URL pro další šablony ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="631eb-145">You can also use [deployment()](resource-group-template-functions-deployment.md#deployment) to get the base URL for the current template, and use that to get the URL for other templates in the same location.</span></span> <span data-ttu-id="631eb-146">Tento přístup je užitečný, pokud se změní vaši polohu šablony (možná z důvodu Správa verzí) nebo chcete vyhnout pevného kódování adresy URL v souboru šablony.</span><span class="sxs-lookup"><span data-stu-id="631eb-146">This approach is useful if your template location changes (maybe due to versioning) or you want to avoid hard coding URLs in the template file.</span></span> 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a><span data-ttu-id="631eb-147">Úplný příklad</span><span class="sxs-lookup"><span data-stu-id="631eb-147">Complete example</span></span>
<span data-ttu-id="631eb-148">Následující příklad šablony zobrazit zjednodušené uspořádání propojených šablon pro ilustraci několik konceptů v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="631eb-148">The following example templates show a simplified arrangement of linked templates to illustrate several of the concepts in this article.</span></span> <span data-ttu-id="631eb-149">Předpokládá se, že šablony byly přidány do kontejneru v účtu úložiště s veřejného přístupu vypnutý.</span><span class="sxs-lookup"><span data-stu-id="631eb-149">It assumes the templates have been added to the same container in a storage account with public access turned off.</span></span> <span data-ttu-id="631eb-150">Propojené šablony předá hodnotu zpět do hlavní šablony **výstupy** části.</span><span class="sxs-lookup"><span data-stu-id="631eb-150">The linked template passes a value back to the main template in the **outputs** section.</span></span>

<span data-ttu-id="631eb-151">**Parent.json** souboru se skládá z:</span><span class="sxs-lookup"><span data-stu-id="631eb-151">The **parent.json** file consists of:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.result.value]"
    }
  }
}
```

<span data-ttu-id="631eb-152">**Helloworld.json** souboru se skládá z:</span><span class="sxs-lookup"><span data-stu-id="631eb-152">The **helloworld.json** file consists of:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
  }
}
```

<span data-ttu-id="631eb-153">V prostředí PowerShell můžete získat token pro kontejner a nasazení šablon s:</span><span class="sxs-lookup"><span data-stu-id="631eb-153">In PowerShell, you get a token for the container and deploy the templates with:</span></span>

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

<span data-ttu-id="631eb-154">V Azure CLI 2.0 můžete získat token pro kontejner a nasazení šablon s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="631eb-154">In Azure CLI 2.0, you get a token for the container and deploy the templates with the following code:</span></span>

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="next-steps"></a><span data-ttu-id="631eb-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="631eb-155">Next steps</span></span>
* <span data-ttu-id="631eb-156">Další informace o definování pořadí nasazení pro vaše prostředky najdete v tématu [definování závislostí v šablonách Azure Resource Manager](resource-group-define-dependencies.md)</span><span class="sxs-lookup"><span data-stu-id="631eb-156">To learn about the defining the deployment order for your resources, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md)</span></span>
* <span data-ttu-id="631eb-157">Zjistěte, jak definovat jeden prostředek ale vytvořit mnoho instancí, najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="631eb-157">To learn how to define one resource but create many instances of it, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>

