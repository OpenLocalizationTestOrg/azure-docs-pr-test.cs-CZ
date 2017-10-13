---
title: "Nasazení aplikace ve škálovací sadě virtuálních počítačů Azure | Dokumentace Microsoftu"
description: "Naučte se nasazovat jednoduché, automaticky škálovatelné aplikace ve škálovací sadě virtuálních počítačů Azure pomocí šablony Azure Resource Manageru."
services: virtual-machine-scale-sets
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: 07883a33382cc660b043c99872312a9e77228253
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-autoscaling-app-using-a-template"></a>Nasazení aplikace s automatickým škálováním pomocí šablony

[Šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) nabízí skvělou možnost pro nasazení skupin souvisejících prostředků. Tento kurz vychází z [Nasazení jednoduché škálovací sady](virtual-machine-scale-sets-mvss-start.md) a popisuje, jak pomocí šablony Azure Resource Manageru nasadit jednoduchou, automaticky škálovatelnou aplikaci ve škálovací sadě.  Automatické škálování můžete nastavit také pomocí PowerShellu, rozhraní příkazového řádku nebo portálu. Další informace najdete v tématu [Přehled automatického škálování](virtual-machine-scale-sets-autoscale-overview.md).

## <a name="two-quickstart-templates"></a>Dvě šablony Quickstart
Když nasadíte škálovací sadu, nový software můžete instalovat na image platformy pomocí [rozšíření virtuálního počítače](../virtual-machines/virtual-machines-windows-extensions-features.md). Rozšíření virtuálního počítače je malá aplikace, která na virtuálních počítačích Azure umožňuje konfiguraci a automatizaci úloh po nasazení, například nasazení aplikace. Ve složce [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) najdete dvě různé ukázkové šablony, díky kterým se dozvíte, jak pomocí rozšíření virtuálního počítači nasadit do škálovací sady automaticky škálovatelnou aplikaci.

### <a name="python-http-server-on-linux"></a>HTTP server s Pythonem v Linuxu
Ukázková šablona [HTTP server s Pythonem v Linuxu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) nasadí jednoduchou, automaticky škálovatelnou aplikaci, která běží ve škálovací sadě systému Linux.  Na každém virtuálním počítači v dané škálovací sadě se pomocí rozšíření virtuálního počítače s přizpůsobeným skriptem nasadí [Bottle](http://bottlepy.org/docs/dev/), webové rozhraní založené na jazyce Python, a jednoduchý server HTTP. Škálovací sada vertikálně navýší kapacitu, když průměrné využití procesoru napříč všemi virtuálními počítači přesáhne 60 %. Pokud průměrné využití procesoru klesne pod 30 %, kapacitu vertikálně sníží.

Ukázková šablona *azuredeploy.json* kromě prostředku škálovací sady určuje také prostředky pro virtuální síť, veřejnou IP adresu, vyrovnávání zatížení a nastavení automatického škálování.  Další informace o vytvoření těchto prostředků v šabloně najdete v tématu [Škálovací sada s automatickým škálováním pro Linux](virtual-machine-scale-sets-linux-autoscale.md).

V šabloně *azuredeploy.json* vlastnost `extensionProfile` prostředku `Microsoft.Compute/virtualMachineScaleSets` specifikuje rozšíření s přizpůsobeným skriptem. `fileUris` určuje umístění skriptu. V tomto případě jde o dva soubory: *workserver.py*, který určujte jednoduchý HTTP server, a *installserver.sh*, který nainstaluje rozhraní Bottle a spustí daný HTTP server. `commandToExecute` určuje příkaz, který se spustí po nasazení škálovací sady.

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "lapextension",
                "properties": {
                  "publisher": "Microsoft.Azure.Extensions",
                  "type": "CustomScript",
                  "typeHandlerVersion": "2.0",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "fileUris": [
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
                    ],
                    "commandToExecute": "bash installserver.sh"
                  }
                }
              }
            ]
          }
```

### <a name="aspnet-mvc-application-on-windows"></a>Aplikace ASP.NET MVC ve Windows
Vzorová šablona [Aplikace ASP.NET MVC ve Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) nasadí do škálovací sady Windows jednoduchou aplikaci ASP.NET MVC, která běží ve službě IIS.  Služba IIS a aplikace MVC se nasazují pomocí rozšíření virtuálního počítače [Požadovaná konfigurace stavu (DSC) PowerShellu](virtual-machine-scale-sets-dsc.md).  Škálovací sada vertikálně navýší kapacitu (postupně u všech instancí virtuálních počítačů), pokud využití procesoru přesáhne 50 % po dobu 5 minut. 

Ukázková šablona *azuredeploy.json* kromě prostředku škálovací sady určuje také prostředky pro virtuální síť, veřejnou IP adresu, vyrovnávání zatížení a nastavení automatického škálování. Tato šablona také ukazuje upgrade aplikace.  Další informace o vytvoření těchto prostředků v šabloně najdete v tématu [Škálovací sada s automatickým škálováním pro Windows](virtual-machine-scale-sets-windows-autoscale.md).

V šabloně *azuredeploy.json* je vlastnost `extensionProfile` prostředku `Microsoft.Compute/virtualMachineScaleSets` určená rozšířením [Požadovaná konfigurace stavu](virtual-machine-scale-sets-dsc.md), které nainstaluje službu IIS a výchozí webovou aplikaci z balíčku WebDeploy.  Skript *IISInstall.ps1* nainstaluje službu IIS na virtuální počítač. Najdete ho ve složce *DSC*.  Webovou aplikaci MVC najdete ve složce *WebDeploy*.  Cesty pro instalaci skriptu a webové aplikace najdete v parametrech `powershelldscZip` a `webDeployPackage` v souboru *azuredeploy.parameters.json*. 

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Powershell.DSC",
                "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.9",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('powershelldscUpdateTagVersion')]",
                  "settings": {
                    "configuration": {
                      "url": "[variables('powershelldscZipFullPath')]",
                      "script": "IISInstall.ps1",
                      "function": "InstallIIS"
                    },
                    "configurationArguments": {
                      "nodeName": "localhost",
                      "WebDeployPackagePath": "[variables('webDeployPackageFullPath')]"
                    }
                  }
                }
              }
            ]
          }
```

## <a name="deploy-the-template"></a>Nasazení šablony
Nejjednodušší způsob, jak nasadit šablonu [HTTP server s Pythonem v Linuxu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) nebo [Aplikace ASP.NET MVC ve Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale), je pomocí tlačítka **Nasazení do Azure**, které najdete na GitHubu v souborech Readme.  Ukázkové šablony můžete nasadit také pomocí PowerShellu nebo rozhraní příkazového řádku Azure CLI.

### <a name="powershell"></a>PowerShell
Zkopírujte soubory [HTTP server s Pythonem v Linuxu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) nebo [Aplikace ASP.NET MVC ve Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) z úložiště GitHub do složky v místním počítači.  Otevřete soubor *azuredeploy.parameters.json* a aktualizujte výchozí hodnoty parametrů `vmssName`, `adminUsername` a `adminPassword`. Následující skript PowerShellu uložte do souboru *deploy.ps1* ve stejné složce, jako je šablona *azuredeploy.json*. Pokud chcete nasadit vzorovou šablonu, v okně pro příkazy PowerShellu spusťte skript *deploy.ps1*.

```powershell
param(
 [Parameter(Mandatory=$True)]
 [string]
 $subscriptionId,

 [Parameter(Mandatory=$True)]
 [string]
 $resourceGroupName,

 [string]
 $resourceGroupLocation,

 [Parameter(Mandatory=$True)]
 [string]
 $deploymentName,

 [string]
 $templateFilePath = "template.json",

 [string]
 $parametersFilePath = "parameters.json"
)

<#
.SYNOPSIS
    Registers RPs
#>
Function RegisterRP {
    Param(
        [string]$ResourceProviderNamespace
    )

    Write-Host "Registering resource provider '$ResourceProviderNamespace'";
    Register-AzureRmResourceProvider -ProviderNamespace $ResourceProviderNamespace;
}

#******************************************************************************
# Script body
# Execution begins here
#******************************************************************************
$ErrorActionPreference = "Stop"

# sign in
Write-Host "Logging in...";
Login-AzureRmAccount;

# select subscription
Write-Host "Selecting subscription '$subscriptionId'";
Select-AzureRmSubscription -SubscriptionID $subscriptionId;

# Register RPs
$resourceProviders = @("microsoft.compute","microsoft.insights","microsoft.network");
if($resourceProviders.length) {
    Write-Host "Registering resource providers"
    foreach($resourceProvider in $resourceProviders) {
        RegisterRP($resourceProvider);
    }
}

#Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
if(!$resourceGroup)
{
    Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start the deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

### <a name="azure-cli"></a>Azure CLI
```azurecli
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# -e: immediately exit if any command has a non-zero exit status
# -o: prevents errors in a pipeline from being masked
# IFS new value is less likely to cause confusing bugs when looping arrays or arguments (e.g. $@)

usage() { echo "Usage: $0 -i <subscriptionId> -g <resourceGroupName> -n <deploymentName> -l <resourceGroupLocation>" 1>&2; exit 1; }

declare subscriptionId=""
declare resourceGroupName=""
declare deploymentName=""
declare resourceGroupLocation=""

# Initialize parameters specified from command line
while getopts ":i:g:n:l:" arg; do
    case "${arg}" in
        i)
            subscriptionId=${OPTARG}
            ;;
        g)
            resourceGroupName=${OPTARG}
            ;;
        n)
            deploymentName=${OPTARG}
            ;;
        l)
            resourceGroupLocation=${OPTARG}
            ;;
        esac
done
shift $((OPTIND-1))

#Prompt for parameters is some required parameters are missing
if [[ -z "$subscriptionId" ]]; then
    echo "Subscription Id:"
    read subscriptionId
    [[ "${subscriptionId:?}" ]]
fi

if [[ -z "$resourceGroupName" ]]; then
    echo "ResourceGroupName:"
    read resourceGroupName
    [[ "${resourceGroupName:?}" ]]
fi

if [[ -z "$deploymentName" ]]; then
    echo "DeploymentName:"
    read deploymentName
fi

if [[ -z "$resourceGroupLocation" ]]; then
    echo "Enter a location below to create a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file to be used
templateFilePath="template.json"

if [ ! -f "$templateFilePath" ]; then
    echo "$templateFilePath not found"
    exit 1
fi

#parameter file path
parametersFilePath="parameters.json"

if [ ! -f "$parametersFilePath" ]; then
    echo "$parametersFilePath not found"
    exit 1
fi

if [ -z "$subscriptionId" ] || [ -z "$resourceGroupName" ] || [ -z "$deploymentName" ]; then
    echo "Either one of subscriptionId, resourceGroupName, deploymentName is empty"
    usage
fi

#login to azure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set the default subscription id
az account set --name $subscriptionId

set +e

#Check for existing RG
az group show $resourceGroupName 1> /dev/null

if [ $? != 0 ]; then
    echo "Resource group with name" $resourceGroupName "could not be found. Creating new resource group.."
    set -e
    (
        set -x
        az group create --name $resourceGroupName --location $resourceGroupLocation 1> /dev/null
    )
    else
    echo "Using existing resource group..."
fi

#Start deployment
echo "Starting deployment..."
(
    set -x
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file $templateFilePath --parameters $parametersFilePath
)

if [ $?  == 0 ];
 then
    echo "Template has been successfully deployed"
fi
```

## <a name="next-steps"></a>Další kroky

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
