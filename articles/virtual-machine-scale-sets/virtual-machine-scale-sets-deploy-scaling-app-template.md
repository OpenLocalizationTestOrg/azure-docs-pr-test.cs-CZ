---
title: "aaaDeploy aplikace na sadu škálování virtuálního počítače Azure | Microsoft Docs"
description: "Přečtěte si toodeploy jednoduché automatické škálování aplikace na škálování virtuálních počítačů, nastavit pomocí šablony Azure Resource Manager."
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
ms.openlocfilehash: 6fccc310312cabfcdddfcbcd2d154fc5cc440417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-autoscaling-app-using-a-template"></a>Nasazení aplikace s automatickým škálováním pomocí šablony

[Šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) jsou skvělý způsob toodeploy skupiny související prostředky. V tomto kurzu vychází [nasadit jednoduchý škálovací sadu](virtual-machine-scale-sets-mvss-start.md) a popisuje, jak toodeploy jednoduché automatické škálování aplikace na škále nastaví pomocí šablony Azure Resource Manager.  Můžete také nastavit automatické škálování pomocí prostředí PowerShell, rozhraní příkazového řádku nebo hello portálu. Další informace najdete v tématu [Přehled automatického škálování](virtual-machine-scale-sets-autoscale-overview.md).

## <a name="two-quickstart-templates"></a>Dvě šablony Quickstart
Když nasadíte škálovací sadu, nový software můžete instalovat na image platformy pomocí [rozšíření virtuálního počítače](../virtual-machines/virtual-machines-windows-extensions-features.md). Rozšíření virtuálního počítače je malá aplikace, která na virtuálních počítačích Azure umožňuje konfiguraci a automatizaci úloh po nasazení, například nasazení aplikace. Dvě různé Ukázka šablony jsou uvedeny v [Azure nebo azure – rychlý start šablony](https://github.com/Azure/azure-quickstart-templates) které ukazují, jak nastavit automatické škálování aplikace na škále toodeploy pomocí rozšíření virtuálního počítače.

### <a name="python-http-server-on-linux"></a>HTTP server s Pythonem v Linuxu
Hello [server Python HTTP v systému Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) šablony ukázka nasadí jednoduché automatické škálování aplikace běžící v škálovací sada Linux.  [Bottle](http://bottlepy.org/docs/dev/), Python webu framework a jednoduchý server HTTP, které jsou nasazeny na každý virtuální počítač ve škálovací hello nastavit pomocí vlastního skriptu rozšíření virtuálního počítače. škálování Hello měřítka nastavit tak, když je větší než 60 % průměrné využití procesoru mezi všechny virtuální počítače a přizpůsobí, pokud využití CPU průměrné hello je méně než 30 %.

Kromě toho toohello sady škálování prostředku hello *azuredeploy.json* Ukázka šablony také deklaruje virtuální sítě, veřejnou IP adresu, nástroj pro vyrovnávání zatížení a škálování nastavení prostředky.  Další informace o vytvoření těchto prostředků v šabloně najdete v tématu [Škálovací sada s automatickým škálováním pro Linux](virtual-machine-scale-sets-linux-autoscale.md).

V hello *azuredeploy.json* šablony, hello `extensionProfile` vlastnost hello `Microsoft.Compute/virtualMachineScaleSets` prostředků určuje rozšíření vlastních skriptů. `fileUris`Určuje umístění skriptů hello. V takovém případě dva soubory: *workserver.py*, která definuje jednoduchý server HTTP, a *installserver.sh*, která nainstaluje Bottle a spustí hello HTTP server. `commandToExecute`Určuje příkaz toorun hello po nasazení hello škálovací sadu.

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
Hello [aplikace ASP.NET MVC v systému Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) šablony ukázka nasadí jednoduchou aplikaci ASP.NET MVC systémem Windows škálovací sadu ve službě IIS.  Službu IIS a hello aplikace MVC jsou nasazeny pomocí hello [požadovaného prostředí PowerShell konfigurace stavu (DSC)](virtual-machine-scale-sets-dsc.md) rozšíření virtuálního počítače.  škálování Hello nastavit měřítka (v instanci virtuálního počítače v čase) Pokud je větší než 50 % využití procesoru po dobu 5 minut. 

Kromě toho toohello sady škálování prostředku hello *azuredeploy.json* Ukázka šablony také deklaruje virtuální sítě, veřejnou IP adresu, nástroj pro vyrovnávání zatížení a škálování nastavení prostředky. Tato šablona také ukazuje upgrade aplikace.  Další informace o vytvoření těchto prostředků v šabloně najdete v tématu [Škálovací sada s automatickým škálováním pro Windows](virtual-machine-scale-sets-windows-autoscale.md).

V hello *azuredeploy.json* šablony, hello `extensionProfile` vlastnost hello `Microsoft.Compute/virtualMachineScaleSets` určuje prostředků [konfigurace požadovaného stavu (DSC)](virtual-machine-scale-sets-dsc.md) rozšíření, který se nainstaluje službu IIS a výchozí webové aplikace z balíčku aplikace WebDeploy.  Hello *IISInstall.ps1* skript nainstaluje službu IIS na virtuálním počítači hello a je nalezena v hello *DSC* složky.  Hello MVC webové aplikace se nachází v hello *WebDeploy* složky.  Hello cesty toohello instalační skript a hello webové aplikace jsou definovány v hello `powershelldscZip` a `webDeployPackage` parametry v hello *azuredeploy.parameters.json* souboru. 

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

## <a name="deploy-hello-template"></a>Nasazení šablony hello
Hello nejjednodušší způsob, jak toodeploy hello [server Python HTTP v systému Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) nebo [aplikace ASP.NET MVC v systému Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) šablona je toouse hello **nasazení tooAzure** nalezeno tlačítko v hello v souborech readme hello v Githubu.  Můžete také použít PowerShell nebo rozhraní příkazového řádku Azure toodeploy hello Ukázka šablony.

### <a name="powershell"></a>PowerShell
Kopírování hello [server Python HTTP v systému Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) nebo [aplikace ASP.NET MVC v systému Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) soubory ze složky tooa úložišti GitHub hello v místním počítači.  Otevřete hello *azuredeploy.parameters.json* souboru a aktualizace hello výchozí hodnoty hello `vmssName`, `adminUsername`, a `adminPassword` parametry. Uložte následující skript prostředí PowerShell příliš hello*deploy.ps1* v hello stejné složce jako hello *azuredeploy.json* šablony. toodeploy hello Ukázka šablony spustit hello *deploy.ps1* skript z příkazové okno prostředí PowerShell.

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
    Write-Host "Resource group '$resourceGroupName' does not exist. toocreate a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start hello deployment
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
# IFS new value is less likely toocause confusing bugs when looping arrays or arguments (e.g. $@)

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
    echo "Enter a location below toocreate a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file toobe used
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

#login tooazure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set hello default subscription id
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
