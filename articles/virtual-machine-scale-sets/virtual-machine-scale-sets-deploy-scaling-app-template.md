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
# <a name="deploy-an-autoscaling-app-using-a-template"></a><span data-ttu-id="7d8f0-103">Nasazení aplikace s automatickým škálováním pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="7d8f0-103">Deploy an autoscaling app using a template</span></span>

<span data-ttu-id="7d8f0-104">[Šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) jsou skvělý způsob toodeploy skupiny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way toodeploy groups of related resources.</span></span> <span data-ttu-id="7d8f0-105">V tomto kurzu vychází [nasadit jednoduchý škálovací sadu](virtual-machine-scale-sets-mvss-start.md) a popisuje, jak toodeploy jednoduché automatické škálování aplikace na škále nastaví pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-105">This tutorial builds on [Deploy a simple scale set](virtual-machine-scale-sets-mvss-start.md) and describes how toodeploy a simple autoscaling application on a scale set using an Azure Resource Manager template.</span></span>  <span data-ttu-id="7d8f0-106">Můžete také nastavit automatické škálování pomocí prostředí PowerShell, rozhraní příkazového řádku nebo hello portálu.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-106">You can also set up autoscaling using PowerShell, CLI, or hello portal.</span></span> <span data-ttu-id="7d8f0-107">Další informace najdete v tématu [Přehled automatického škálování](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7d8f0-107">For more information, see [Autoscale overview](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

## <a name="two-quickstart-templates"></a><span data-ttu-id="7d8f0-108">Dvě šablony Quickstart</span><span class="sxs-lookup"><span data-stu-id="7d8f0-108">Two quickstart templates</span></span>
<span data-ttu-id="7d8f0-109">Když nasadíte škálovací sadu, nový software můžete instalovat na image platformy pomocí [rozšíření virtuálního počítače](../virtual-machines/virtual-machines-windows-extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="7d8f0-109">When you deploy a scale set you can install new software on a platform image using a [VM Extension](../virtual-machines/virtual-machines-windows-extensions-features.md).</span></span> <span data-ttu-id="7d8f0-110">Rozšíření virtuálního počítače je malá aplikace, která na virtuálních počítačích Azure umožňuje konfiguraci a automatizaci úloh po nasazení, například nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-110">A VM extension is a small application that provides post-deployment configuration and automation tasks on Azure virtual machines, such as deploying an app.</span></span> <span data-ttu-id="7d8f0-111">Dvě různé Ukázka šablony jsou uvedeny v [Azure nebo azure – rychlý start šablony](https://github.com/Azure/azure-quickstart-templates) které ukazují, jak nastavit automatické škálování aplikace na škále toodeploy pomocí rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-111">Two different sample templates are provided in [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) which show how toodeploy an autoscaling application onto a scale set using VM extensions.</span></span>

### <a name="python-http-server-on-linux"></a><span data-ttu-id="7d8f0-112">HTTP server s Pythonem v Linuxu</span><span class="sxs-lookup"><span data-stu-id="7d8f0-112">Python HTTP server on Linux</span></span>
<span data-ttu-id="7d8f0-113">Hello [server Python HTTP v systému Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) šablony ukázka nasadí jednoduché automatické škálování aplikace běžící v škálovací sada Linux.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-113">hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sample template deploys a simple autoscaling application running on a Linux scale set.</span></span>  <span data-ttu-id="7d8f0-114">[Bottle](http://bottlepy.org/docs/dev/), Python webu framework a jednoduchý server HTTP, které jsou nasazeny na každý virtuální počítač ve škálovací hello nastavit pomocí vlastního skriptu rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-114">[Bottle](http://bottlepy.org/docs/dev/), a Python web framework, and a simple HTTP server are deployed on each VM in hello scale set using a custom script VM extension.</span></span> <span data-ttu-id="7d8f0-115">škálování Hello měřítka nastavit tak, když je větší než 60 % průměrné využití procesoru mezi všechny virtuální počítače a přizpůsobí, pokud využití CPU průměrné hello je méně než 30 %.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-115">hello scale set scales up when average CPU utilization across all VMs is greater than 60% and scales down when hello average CPU utilization is less than 30%.</span></span>

<span data-ttu-id="7d8f0-116">Kromě toho toohello sady škálování prostředku hello *azuredeploy.json* Ukázka šablony také deklaruje virtuální sítě, veřejnou IP adresu, nástroj pro vyrovnávání zatížení a škálování nastavení prostředky.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-116">In addition toohello scale set resource, hello *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span>  <span data-ttu-id="7d8f0-117">Další informace o vytvoření těchto prostředků v šabloně najdete v tématu [Škálovací sada s automatickým škálováním pro Linux](virtual-machine-scale-sets-linux-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="7d8f0-117">For more information on creating these resources in a template, see [Linux scale set with autoscale](virtual-machine-scale-sets-linux-autoscale.md).</span></span>

<span data-ttu-id="7d8f0-118">V hello *azuredeploy.json* šablony, hello `extensionProfile` vlastnost hello `Microsoft.Compute/virtualMachineScaleSets` prostředků určuje rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-118">In hello *azuredeploy.json* template, hello `extensionProfile` property of hello `Microsoft.Compute/virtualMachineScaleSets` resource specifies a custom script extension.</span></span> <span data-ttu-id="7d8f0-119">`fileUris`Určuje umístění skriptů hello.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-119">`fileUris` specifies hello script(s) location.</span></span> <span data-ttu-id="7d8f0-120">V takovém případě dva soubory: *workserver.py*, která definuje jednoduchý server HTTP, a *installserver.sh*, která nainstaluje Bottle a spustí hello HTTP server.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-120">In this case, two files: *workserver.py*, which defines a simple HTTP server, and *installserver.sh*, which installs Bottle and starts hello HTTP server.</span></span> <span data-ttu-id="7d8f0-121">`commandToExecute`Určuje příkaz toorun hello po nasazení hello škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-121">`commandToExecute` specifies hello command toorun after hello scale set has been deployed.</span></span>

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

### <a name="aspnet-mvc-application-on-windows"></a><span data-ttu-id="7d8f0-122">Aplikace ASP.NET MVC ve Windows</span><span class="sxs-lookup"><span data-stu-id="7d8f0-122">ASP.NET MVC application on Windows</span></span>
<span data-ttu-id="7d8f0-123">Hello [aplikace ASP.NET MVC v systému Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) šablony ukázka nasadí jednoduchou aplikaci ASP.NET MVC systémem Windows škálovací sadu ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-123">hello [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) sample template deploys a simple ASP.NET MVC app running in IIS on Windows scale set.</span></span>  <span data-ttu-id="7d8f0-124">Službu IIS a hello aplikace MVC jsou nasazeny pomocí hello [požadovaného prostředí PowerShell konfigurace stavu (DSC)](virtual-machine-scale-sets-dsc.md) rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-124">IIS and hello MVC app are deployed using hello [PowerShell desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) VM extension.</span></span>  <span data-ttu-id="7d8f0-125">škálování Hello nastavit měřítka (v instanci virtuálního počítače v čase) Pokud je větší než 50 % využití procesoru po dobu 5 minut.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-125">hello scale set scales up (on VM instance at a time) when CPU utilization is greater than 50% for 5 minutes.</span></span> 

<span data-ttu-id="7d8f0-126">Kromě toho toohello sady škálování prostředku hello *azuredeploy.json* Ukázka šablony také deklaruje virtuální sítě, veřejnou IP adresu, nástroj pro vyrovnávání zatížení a škálování nastavení prostředky.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-126">In addition toohello scale set resource, hello *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span> <span data-ttu-id="7d8f0-127">Tato šablona také ukazuje upgrade aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-127">This template also demonstrates application upgrade.</span></span>  <span data-ttu-id="7d8f0-128">Další informace o vytvoření těchto prostředků v šabloně najdete v tématu [Škálovací sada s automatickým škálováním pro Windows](virtual-machine-scale-sets-windows-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="7d8f0-128">For more information on creating these resources in a template, see [Windows scale set with autoscale](virtual-machine-scale-sets-windows-autoscale.md).</span></span>

<span data-ttu-id="7d8f0-129">V hello *azuredeploy.json* šablony, hello `extensionProfile` vlastnost hello `Microsoft.Compute/virtualMachineScaleSets` určuje prostředků [konfigurace požadovaného stavu (DSC)](virtual-machine-scale-sets-dsc.md) rozšíření, který se nainstaluje službu IIS a výchozí webové aplikace z balíčku aplikace WebDeploy.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-129">In hello *azuredeploy.json* template, hello `extensionProfile` property of hello `Microsoft.Compute/virtualMachineScaleSets` resource specifies a [desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) extension which installs IIS and a default web app from a WebDeploy package.</span></span>  <span data-ttu-id="7d8f0-130">Hello *IISInstall.ps1* skript nainstaluje službu IIS na virtuálním počítači hello a je nalezena v hello *DSC* složky.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-130">hello *IISInstall.ps1* script installs IIS on hello virtual machine and is found in hello *DSC* folder.</span></span>  <span data-ttu-id="7d8f0-131">Hello MVC webové aplikace se nachází v hello *WebDeploy* složky.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-131">hello MVC web app is found in hello *WebDeploy* folder.</span></span>  <span data-ttu-id="7d8f0-132">Hello cesty toohello instalační skript a hello webové aplikace jsou definovány v hello `powershelldscZip` a `webDeployPackage` parametry v hello *azuredeploy.parameters.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-132">hello paths toohello install script and hello web app are defined in hello `powershelldscZip` and `webDeployPackage` parameters in hello *azuredeploy.parameters.json* file.</span></span> 

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

## <a name="deploy-hello-template"></a><span data-ttu-id="7d8f0-133">Nasazení šablony hello</span><span class="sxs-lookup"><span data-stu-id="7d8f0-133">Deploy hello template</span></span>
<span data-ttu-id="7d8f0-134">Hello nejjednodušší způsob, jak toodeploy hello [server Python HTTP v systému Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) nebo [aplikace ASP.NET MVC v systému Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) šablona je toouse hello **nasazení tooAzure** nalezeno tlačítko v hello v souborech readme hello v Githubu.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-134">hello simplest way toodeploy hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) template is toouse hello **Deploy tooAzure** button found in hello in hello readme files in GitHub.</span></span>  <span data-ttu-id="7d8f0-135">Můžete také použít PowerShell nebo rozhraní příkazového řádku Azure toodeploy hello Ukázka šablony.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-135">You can also use PowerShell or Azure CLI toodeploy hello sample templates.</span></span>

### <a name="powershell"></a><span data-ttu-id="7d8f0-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7d8f0-136">PowerShell</span></span>
<span data-ttu-id="7d8f0-137">Kopírování hello [server Python HTTP v systému Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) nebo [aplikace ASP.NET MVC v systému Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) soubory ze složky tooa úložišti GitHub hello v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-137">Copy hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) files from hello GitHub repo tooa folder on your local computer.</span></span>  <span data-ttu-id="7d8f0-138">Otevřete hello *azuredeploy.parameters.json* souboru a aktualizace hello výchozí hodnoty hello `vmssName`, `adminUsername`, a `adminPassword` parametry.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-138">Open hello *azuredeploy.parameters.json* file and update hello default values of hello `vmssName`, `adminUsername`, and `adminPassword` parameters.</span></span> <span data-ttu-id="7d8f0-139">Uložte následující skript prostředí PowerShell příliš hello*deploy.ps1* v hello stejné složce jako hello *azuredeploy.json* šablony.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-139">Save hello following PowerShell script too*deploy.ps1* in hello same folder as hello *azuredeploy.json* template.</span></span> <span data-ttu-id="7d8f0-140">toodeploy hello Ukázka šablony spustit hello *deploy.ps1* skript z příkazové okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7d8f0-140">toodeploy hello sample template run hello *deploy.ps1* script from a PowerShell command window.</span></span>

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

### <a name="azure-cli"></a><span data-ttu-id="7d8f0-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7d8f0-141">Azure CLI</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="7d8f0-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d8f0-142">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
