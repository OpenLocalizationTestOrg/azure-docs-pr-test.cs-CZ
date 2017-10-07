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
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Použití propojených šablon při nasazování prostředků Azure
Z v rámci jedné šablony Azure Resource Manager, můžete se propojit tooanother šablony, která vám umožní toodecompose nasazení na sadu cílových, zaměřené na konkrétní účel šablony. Stejně jako u decomposing aplikace do více tříd kódu, poskytuje rozložením výhody z hlediska testování, opakované použití a přehlednosti.  

Můžete předat parametry ze šablony propojené tooa hlavní šablonu, a tyto parametry můžete namapovat přímo tooparameters nebo proměnné vystavené hello volání šablony. Hello propojené šablony můžete předat také šablonu výstup proměnné back toohello zdroje, povolení výměna obousměrný dat mezi šablonami.

## <a name="linking-tooa-template"></a>Propojování tooa šablony
Můžete vytvořit propojení mezi přidáním nasazení prostředků v rámci šablony hello hlavní body propojené šablona toohello dvě šablony. Nastavit hello **templateLink** toohello vlastnost URI hello propojené šablony. Zadáním hodnot parametru šablony hello propojené přímo v šabloně nebo v souboru parametrů. Hello následující příklad používá hello **parametry** vlastnost toospecify přímo hodnotu parametru.

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

Podobně jako ostatní typy prostředků můžete nastavit závislosti mezi hello propojené šablony a dalším prostředkům. Proto pokud další prostředky vyžadují hodnotu výstup z hello propojené šablony, jste měli jistotu, že propojené šablonu hello je nasadit před sebou. Nebo, když propojené šablony hello závisí na jiné prostředky, můžete zajistit, že jiné prostředky, které jsou nasazeny před hello propojené šablony. Můžete načíst hodnotu z šablonu propojené s hello následující syntaxi:

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

Hello služby Správce prostředků musí být schopný tooaccess hello propojené šablony. Nelze zadat místní soubor nebo soubor, který je k dispozici ve vaší místní síti hello propojené šablony. Můžete pouze zadat hodnotu identifikátoru URI, která zahrnuje buď **http** nebo **https**. Jednou z možností je tooplace propojené šablony na účet úložiště a použít hello identifikátor URI pro tuto položku, jako ukazuje následující příklad hello:

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

I když hello propojené šablony musí být externě k dispozici, není nutné toobe všeobecně dostupná toohello veřejné. Můžete přidat vašeho účtu úložiště privátní tooa šablony, které je vlastník účtu úložiště přístupné tooonly hello. Pak vytvořte pro přístup k sdílený přístupový podpis (SAS) tokenu tooenable během nasazení. Můžete přidat tento SAS token toohello URI hello propojené šablony. Postup nastavení šablony v účtu úložiště a generování tokenu SAS naleznete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md) nebo [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](resource-group-template-deploy-cli.md). 

Hello následující příklad ukazuje šablonu nadřazené šablona tooanother odkazy. propojené šablony Hello přistupuje s tokenem SAS, který se předává v jako parametr.

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

I když hello token, je předaná jako zabezpečený řetězec, hello URI hello propojené šablony, včetně hello tokenu SAS, je přihlášen hello operace nasazení. ohrožení toolimit nastavit vypršení platnosti pro hello token.

Správce prostředků zpracovává každé propojené šablony jako samostatné nasazení. V historii hello nasazení pro skupinu prostředků hello zobrazí samostatná nasazení pro nadřazené hello a vnořené šablony.

![historie nasazení](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-tooa-parameter-file"></a>Soubor parametrů tooa propojení
Další příklad Hello používá hello **parametersLink** vlastnost toolink tooa parametr souboru.

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

Hello URI hodnota hello propojené parametr souboru nemůže být místní soubor a musí obsahovat buď **http** nebo **https**. soubor parametrů Hello může být také omezené tooaccess prostřednictvím tokenu SAS.

## <a name="using-variables-toolink-templates"></a>Pomocí šablon toolink proměnné
Hello předchozí příklady nám ukázaly pevně definovaných hodnot adresu URL pro hello šablony odkazy. Tento přístup může fungovat pro jednoduchou šablonu, ale nebude fungovat správně při práci s velké sady modulární šablony. Místo toho můžete vytvořit statickou proměnné, která ukládá základní adresu URL pro hlavní šablonu hello a dynamicky vytvářet adresy URL pro hello propojené šablony z této základní adresu URL. Hello výhodou tohoto přístupu je, že můžete snadno přesunout nebo rozvětvení hello šablonu protože potřebujete jenom toochange hello statické proměnné v šabloně hlavní hello. hlavní šablonu Hello předá hello správné že identifikátory URI v rámci hello rozložit šablony.

Hello následující příklad ukazuje, jak toouse základní adresa URL toocreate dvou adres URL pro propojené šablony (**sharedTemplateUrl** a **vmTemplate**). 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Můžete také použít [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello základní adresu URL pro aktuální šablony hello a použít tuto adresu URL hello tooget pro další šablony v hello stejné umístění. Tento přístup je užitečný, pokud se změní umístění vaší šablony (možná kvůli tooversioning) nebo chcete tooavoid pevné kódování adresy URL v souboru šablony hello. 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a>Úplný příklad
Následující příklad šablony Hello zobrazit zjednodušené uspořádání propojených šablon tooillustrate několik konceptů hello v tomto článku. Předpokládá se, že šablony hello přidané toohello stejnému kontejneru v účtu úložiště s veřejného přístupu vypnutý. propojené šablony Hello předá hodnotu hlavní šablonu back toohello hello **výstupy** části.

Hello **parent.json** souboru se skládá z:

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

Hello **helloworld.json** souboru se skládá z:

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

V prostředí PowerShell můžete získat token pro kontejner hello a nasadit hello šablon s:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

V Azure CLI 2.0 můžete získat token pro kontejner hello a nasazení šablon hello hello následující kód:

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

## <a name="next-steps"></a>Další kroky
* toolearn o hello definování hello pořadím nasazení pro vaše prostředky, najdete v části [definování závislostí v šablonách Azure Resource Manager](resource-group-define-dependencies.md)
* toolearn způsobu toodefine jeden prostředek ale vytvořit velký počet instancí, najdete v [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md)

