---
title: "aaaLink šablony pro nasazení Azure | Microsoft Docs"
description: "Popisuje, jak toouse propojené šablony v toocreate šablony Azure Resource Manager modulární šablonu řešení. Popisuje, jak toopass hodnot parametrů, zadejte parametr soubor a dynamicky vytvoření adresy URL."
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
ms.openlocfilehash: b935b1810db5ce894d009403cd4bb945cab34ba7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a><span data-ttu-id="3bf8a-104">Použití propojených šablon při nasazování prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="3bf8a-104">Using linked templates when deploying Azure resources</span></span>
<span data-ttu-id="3bf8a-105">Z v rámci jedné šablony Azure Resource Manager, můžete se propojit tooanother šablony, která vám umožní toodecompose nasazení na sadu cílových, zaměřené na konkrétní účel šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-105">From within one Azure Resource Manager template, you can link tooanother template, which enables you toodecompose your deployment into a set of targeted, purpose-specific templates.</span></span> <span data-ttu-id="3bf8a-106">Stejně jako u decomposing aplikace do více tříd kódu, poskytuje rozložením výhody z hlediska testování, opakované použití a přehlednosti.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-106">As with decomposing an application into several code classes, decomposition provides benefits in terms of testing, reuse, and readability.</span></span>  

<span data-ttu-id="3bf8a-107">Můžete předat parametry ze šablony propojené tooa hlavní šablonu, a tyto parametry můžete namapovat přímo tooparameters nebo proměnné vystavené hello volání šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-107">You can pass parameters from a main template tooa linked template, and those parameters can directly map tooparameters or variables exposed by hello calling template.</span></span> <span data-ttu-id="3bf8a-108">Hello propojené šablony můžete předat také šablonu výstup proměnné back toohello zdroje, povolení výměna obousměrný dat mezi šablonami.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-108">hello linked template can also pass an output variable back toohello source template, enabling a two-way data exchange between templates.</span></span>

## <a name="linking-tooa-template"></a><span data-ttu-id="3bf8a-109">Propojování tooa šablony</span><span class="sxs-lookup"><span data-stu-id="3bf8a-109">Linking tooa template</span></span>
<span data-ttu-id="3bf8a-110">Můžete vytvořit propojení mezi přidáním nasazení prostředků v rámci šablony hello hlavní body propojené šablona toohello dvě šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-110">You create a link between two templates by adding a deployment resource within hello main template that points toohello linked template.</span></span> <span data-ttu-id="3bf8a-111">Nastavit hello **templateLink** toohello vlastnost URI hello propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-111">You set hello **templateLink** property toohello URI of hello linked template.</span></span> <span data-ttu-id="3bf8a-112">Zadáním hodnot parametru šablony hello propojené přímo v šabloně nebo v souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-112">You can provide parameter values for hello linked template directly in your template or in a parameter file.</span></span> <span data-ttu-id="3bf8a-113">Hello následující příklad používá hello **parametry** vlastnost toospecify přímo hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-113">hello following example uses hello **parameters** property toospecify a parameter value directly.</span></span>

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

<span data-ttu-id="3bf8a-114">Podobně jako ostatní typy prostředků můžete nastavit závislosti mezi hello propojené šablony a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-114">Like other resource types, you can set dependencies between hello linked template and other resources.</span></span> <span data-ttu-id="3bf8a-115">Proto pokud další prostředky vyžadují hodnotu výstup z hello propojené šablony, jste měli jistotu, že propojené šablonu hello je nasadit před sebou.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-115">Therefore, when other resources require an output value from hello linked template, you can make sure hello linked template is deployed before them.</span></span> <span data-ttu-id="3bf8a-116">Nebo, když propojené šablony hello závisí na jiné prostředky, můžete zajistit, že jiné prostředky, které jsou nasazeny před hello propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-116">Or, when hello linked template relies on other resources, you can make sure other resources are deployed before hello linked template.</span></span> <span data-ttu-id="3bf8a-117">Můžete načíst hodnotu z šablonu propojené s hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="3bf8a-117">You can retrieve a value from a linked template with hello following syntax:</span></span>

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

<span data-ttu-id="3bf8a-118">Hello služby Správce prostředků musí být schopný tooaccess hello propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-118">hello Resource Manager service must be able tooaccess hello linked template.</span></span> <span data-ttu-id="3bf8a-119">Nelze zadat místní soubor nebo soubor, který je k dispozici ve vaší místní síti hello propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-119">You cannot specify a local file or a file that is only available on your local network for hello linked template.</span></span> <span data-ttu-id="3bf8a-120">Můžete pouze zadat hodnotu identifikátoru URI, která zahrnuje buď **http** nebo **https**.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-120">You can only provide a URI value that includes either **http** or **https**.</span></span> <span data-ttu-id="3bf8a-121">Jednou z možností je tooplace propojené šablony na účet úložiště a použít hello identifikátor URI pro tuto položku, jako ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="3bf8a-121">One option is tooplace your linked template in a storage account, and use hello URI for that item, such as shown in hello following example:</span></span>

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

<span data-ttu-id="3bf8a-122">I když hello propojené šablony musí být externě k dispozici, není nutné toobe všeobecně dostupná toohello veřejné.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-122">Although hello linked template must be externally available, it does not need toobe generally available toohello public.</span></span> <span data-ttu-id="3bf8a-123">Můžete přidat vašeho účtu úložiště privátní tooa šablony, které je vlastník účtu úložiště přístupné tooonly hello.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-123">You can add your template tooa private storage account that is accessible tooonly hello storage account owner.</span></span> <span data-ttu-id="3bf8a-124">Pak vytvořte pro přístup k sdílený přístupový podpis (SAS) tokenu tooenable během nasazení.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-124">Then, you create a shared access signature (SAS) token tooenable access during deployment.</span></span> <span data-ttu-id="3bf8a-125">Můžete přidat tento SAS token toohello URI hello propojené šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-125">You add that SAS token toohello URI for hello linked template.</span></span> <span data-ttu-id="3bf8a-126">Postup nastavení šablony v účtu úložiště a generování tokenu SAS naleznete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md) nebo [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3bf8a-126">For steps on setting up a template in a storage account and generating a SAS token, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md) or [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> 

<span data-ttu-id="3bf8a-127">Hello následující příklad ukazuje šablonu nadřazené šablona tooanother odkazy.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-127">hello following example shows a parent template that links tooanother template.</span></span> <span data-ttu-id="3bf8a-128">propojené šablony Hello přistupuje s tokenem SAS, který se předává v jako parametr.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-128">hello linked template is accessed with a SAS token that is passed in as a parameter.</span></span>

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

<span data-ttu-id="3bf8a-129">I když hello token, je předaná jako zabezpečený řetězec, hello URI hello propojené šablony, včetně hello tokenu SAS, je přihlášen hello operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-129">Even though hello token is passed in as a secure string, hello URI of hello linked template, including hello SAS token, is logged in hello deployment operations.</span></span> <span data-ttu-id="3bf8a-130">ohrožení toolimit nastavit vypršení platnosti pro hello token.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-130">toolimit exposure, set an expiration for hello token.</span></span>

<span data-ttu-id="3bf8a-131">Správce prostředků zpracovává každé propojené šablony jako samostatné nasazení.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-131">Resource Manager handles each linked template as a separate deployment.</span></span> <span data-ttu-id="3bf8a-132">V historii hello nasazení pro skupinu prostředků hello zobrazí samostatná nasazení pro nadřazené hello a vnořené šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-132">In hello deployment history for hello resource group, you see separate deployments for hello parent and nested templates.</span></span>

![historie nasazení](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-tooa-parameter-file"></a><span data-ttu-id="3bf8a-134">Soubor parametrů tooa propojení</span><span class="sxs-lookup"><span data-stu-id="3bf8a-134">Linking tooa parameter file</span></span>
<span data-ttu-id="3bf8a-135">Další příklad Hello používá hello **parametersLink** vlastnost toolink tooa parametr souboru.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-135">hello next example uses hello **parametersLink** property toolink tooa parameter file.</span></span>

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

<span data-ttu-id="3bf8a-136">Hello URI hodnota hello propojené parametr souboru nemůže být místní soubor a musí obsahovat buď **http** nebo **https**.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-136">hello URI value for hello linked parameter file cannot be a local file, and must include either **http** or **https**.</span></span> <span data-ttu-id="3bf8a-137">soubor parametrů Hello může být také omezené tooaccess prostřednictvím tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-137">hello parameter file can also be limited tooaccess through a SAS token.</span></span>

## <a name="using-variables-toolink-templates"></a><span data-ttu-id="3bf8a-138">Pomocí šablon toolink proměnné</span><span class="sxs-lookup"><span data-stu-id="3bf8a-138">Using variables toolink templates</span></span>
<span data-ttu-id="3bf8a-139">Hello předchozí příklady nám ukázaly pevně definovaných hodnot adresu URL pro hello šablony odkazy.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-139">hello previous examples showed hard-coded URL values for hello template links.</span></span> <span data-ttu-id="3bf8a-140">Tento přístup může fungovat pro jednoduchou šablonu, ale nebude fungovat správně při práci s velké sady modulární šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-140">This approach might work for a simple template but it does not work well when working with a large set of modular templates.</span></span> <span data-ttu-id="3bf8a-141">Místo toho můžete vytvořit statickou proměnné, která ukládá základní adresu URL pro hlavní šablonu hello a dynamicky vytvářet adresy URL pro hello propojené šablony z této základní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-141">Instead, you can create a static variable that stores a base URL for hello main template and then dynamically create URLs for hello linked templates from that base URL.</span></span> <span data-ttu-id="3bf8a-142">Hello výhodou tohoto přístupu je, že můžete snadno přesunout nebo rozvětvení hello šablonu protože potřebujete jenom toochange hello statické proměnné v šabloně hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-142">hello benefit of this approach is you can easily move or fork hello template because you only need toochange hello static variable in hello main template.</span></span> <span data-ttu-id="3bf8a-143">hlavní šablonu Hello předá hello správné že identifikátory URI v rámci hello rozložit šablony.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-143">hello main template passes hello correct URIs throughout hello decomposed template.</span></span>

<span data-ttu-id="3bf8a-144">Hello následující příklad ukazuje, jak toouse základní adresa URL toocreate dvou adres URL pro propojené šablony (**sharedTemplateUrl** a **vmTemplate**).</span><span class="sxs-lookup"><span data-stu-id="3bf8a-144">hello following example shows how toouse a base URL toocreate two URLs for linked templates (**sharedTemplateUrl** and **vmTemplate**).</span></span> 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

<span data-ttu-id="3bf8a-145">Můžete také použít [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello základní adresu URL pro aktuální šablony hello a použít tuto adresu URL hello tooget pro další šablony v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-145">You can also use [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello base URL for hello current template, and use that tooget hello URL for other templates in hello same location.</span></span> <span data-ttu-id="3bf8a-146">Tento přístup je užitečný, pokud se změní umístění vaší šablony (možná kvůli tooversioning) nebo chcete tooavoid pevné kódování adresy URL v souboru šablony hello.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-146">This approach is useful if your template location changes (maybe due tooversioning) or you want tooavoid hard coding URLs in hello template file.</span></span> 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a><span data-ttu-id="3bf8a-147">Úplný příklad</span><span class="sxs-lookup"><span data-stu-id="3bf8a-147">Complete example</span></span>
<span data-ttu-id="3bf8a-148">Následující příklad šablony Hello zobrazit zjednodušené uspořádání propojených šablon tooillustrate několik konceptů hello v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-148">hello following example templates show a simplified arrangement of linked templates tooillustrate several of hello concepts in this article.</span></span> <span data-ttu-id="3bf8a-149">Předpokládá se, že šablony hello přidané toohello stejnému kontejneru v účtu úložiště s veřejného přístupu vypnutý.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-149">It assumes hello templates have been added toohello same container in a storage account with public access turned off.</span></span> <span data-ttu-id="3bf8a-150">propojené šablony Hello předá hodnotu hlavní šablonu back toohello hello **výstupy** části.</span><span class="sxs-lookup"><span data-stu-id="3bf8a-150">hello linked template passes a value back toohello main template in hello **outputs** section.</span></span>

<span data-ttu-id="3bf8a-151">Hello **parent.json** souboru se skládá z:</span><span class="sxs-lookup"><span data-stu-id="3bf8a-151">hello **parent.json** file consists of:</span></span>

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

<span data-ttu-id="3bf8a-152">Hello **helloworld.json** souboru se skládá z:</span><span class="sxs-lookup"><span data-stu-id="3bf8a-152">hello **helloworld.json** file consists of:</span></span>

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

<span data-ttu-id="3bf8a-153">V prostředí PowerShell můžete získat token pro kontejner hello a nasadit hello šablon s:</span><span class="sxs-lookup"><span data-stu-id="3bf8a-153">In PowerShell, you get a token for hello container and deploy hello templates with:</span></span>

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

<span data-ttu-id="3bf8a-154">V Azure CLI 2.0 můžete získat token pro kontejner hello a nasazení šablon hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="3bf8a-154">In Azure CLI 2.0, you get a token for hello container and deploy hello templates with hello following code:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="3bf8a-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3bf8a-155">Next steps</span></span>
* <span data-ttu-id="3bf8a-156">toolearn o hello definování hello pořadím nasazení pro vaše prostředky, najdete v části [definování závislostí v šablonách Azure Resource Manager](resource-group-define-dependencies.md)</span><span class="sxs-lookup"><span data-stu-id="3bf8a-156">toolearn about hello defining hello deployment order for your resources, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md)</span></span>
* <span data-ttu-id="3bf8a-157">toolearn způsobu toodefine jeden prostředek ale vytvořit velký počet instancí, najdete v [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="3bf8a-157">toolearn how toodefine one resource but create many instances of it, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>

