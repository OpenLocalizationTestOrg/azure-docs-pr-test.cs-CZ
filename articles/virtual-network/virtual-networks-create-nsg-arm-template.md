---
title: "Vytvoření skupin zabezpečení sítě - šablony Azure Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak vytvořit a nasadit skupin zabezpečení sítě pomocí šablony Azure Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: f3e7385d-717c-44ff-be20-f9aa450aa99b
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 88f7e5b2144daee7bf1c8e7312ba98e6fa967899
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-an-azure-resource-manager-template"></a><span data-ttu-id="5d1c6-103">Vytvořit síť pomocí šablony Azure Resource Manager skupin zabezpečení</span><span class="sxs-lookup"><span data-stu-id="5d1c6-103">Create network security groups using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="5d1c6-104">Tento článek se týká modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-104">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="5d1c6-105">Můžete také [vytvářet skupiny Nsg v modelu nasazení classic](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5d1c6-105">You can also [create NSGs in the classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a><span data-ttu-id="5d1c6-106">Skupina NSG prostředky v souboru šablony</span><span class="sxs-lookup"><span data-stu-id="5d1c6-106">NSG resources in a template file</span></span>
<span data-ttu-id="5d1c6-107">Můžete zobrazit a stáhnout [Ukázka šablony](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span><span class="sxs-lookup"><span data-stu-id="5d1c6-107">You can view and download the [sample template](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span></span>

<span data-ttu-id="5d1c6-108">V následující části zobrazuje definici front-end NSG, závislosti na scénáři.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-108">The following section shows the definition of the front-end NSG, based on the scenario.</span></span>

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Network/networkSecurityGroups",
"name": "[parameters('frontEndNSGName')]",
"location": "[resourceGroup().location]",
"tags": {
  "displayName": "NSG - Front End"
},
"properties": {
  "securityRules": [
    {
      "name": "rdp-rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 100,
        "direction": "Inbound"
      }
    },
    {
      "name": "web-rule",
      "properties": {
        "description": "Allow WEB",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 101,
        "direction": "Inbound"
      }
    }
  ]
}
```
<span data-ttu-id="5d1c6-109">Přidružení skupiny NSG k podsíti front-endu, budete muset změnit definici podsítě v šabloně a použít odkaz na id skupiny nsg.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-109">To associate the NSG to the front-end subnet, you have to change the subnet definition in the template, and use the reference id for the NSG.</span></span>

```json
"subnets": [
  {
    "name": "[parameters('frontEndSubnetName')]",
    "properties": {
      "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
      "networkSecurityGroup": {
      "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
      }
    }
  }, 
```

<span data-ttu-id="5d1c6-110">Všimněte si stejné prováděná pro NSG back-end a back-end podsíť v šabloně.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-110">Notice the same being done for the back-end NSG and the back-end subnet in the template.</span></span>

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a><span data-ttu-id="5d1c6-111">Nasazení šablony ARM pomocí metody Click to Deploy</span><span class="sxs-lookup"><span data-stu-id="5d1c6-111">Deploy the ARM template by using click to deploy</span></span>
<span data-ttu-id="5d1c6-112">Ukázková šablona, která je k dispozici ve veřejném úložišti, používá soubor parametrů obsahující výchozí hodnoty, které se použijí k vygenerování výše popsaného scénáře.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-112">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="5d1c6-113">Pokud chcete nasadit tuto šablonu pomocí metody Click to Deploy, pokračujte na [tento odkaz](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klikněte na **Nasadit do Azure**, v případě potřeby nahraďte výchozí hodnoty parametrů, a pokračujte podle pokynů na portálu.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-113">To deploy this template using click to deploy, follow [this link](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

## <a name="deploy-the-arm-template-by-using-powershell"></a><span data-ttu-id="5d1c6-114">Nasazení šablony ARM pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5d1c6-114">Deploy the ARM template by using PowerShell</span></span>
<span data-ttu-id="5d1c6-115">Pokud chcete nasadit šablonu ARM, kterou jste stáhli, pomocí prostředí PowerShell, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-115">To deploy the ARM template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="5d1c6-116">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, postupujte podle pokynů [způsob instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) nainstalovat a nakonfigurovat ho.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-116">If you have never used Azure PowerShell, follow the instructions in the [How to Install and Configure Azure PowerShell](/powershell/azure/overview) to install and configure it.</span></span>
2. <span data-ttu-id="5d1c6-117">Spustit  **`New-AzureRmResourceGroup`**  vytvořte skupinu prostředků pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-117">Run the **`New-AzureRmResourceGroup`** cmdlet to create a resource group using the template.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location uswest `
    -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
    -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="5d1c6-118">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5d1c6-118">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  
   
        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  
   
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a><span data-ttu-id="5d1c6-119">Nasazení šablony ARM pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5d1c6-119">Deploy the ARM template by using the Azure CLI</span></span>
<span data-ttu-id="5d1c6-120">Nasazení šablony ARM pomocí rozhraní příkazového řádku Azure, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-120">To deploy the ARM template by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="5d1c6-121">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-121">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="5d1c6-122">Spuštěním příkazu **`azure config mode`** přepněte do režimu Resource Manager, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-122">Run the **`azure config mode`** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="5d1c6-123">Toto je očekávaný výstup příkazu:</span><span class="sxs-lookup"><span data-stu-id="5d1c6-123">The following is the expected output for the command:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="5d1c6-124">Spuštěním rutiny **`azure group deployment create`** nasadíte novou síť VNet pomocí šablony a souborů parametrů, které jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-124">Run the **`azure group deployment create`** cmdlet to deploy the new VNet by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="5d1c6-125">Seznam uvedený za výstupem vysvětluje použité parametry.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-125">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="5d1c6-126">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="5d1c6-126">Expected output:</span></span>
   
        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK
   
   * <span data-ttu-id="5d1c6-127">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-127">**-n (or --name)**.</span></span> <span data-ttu-id="5d1c6-128">Název skupiny prostředků, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-128">Name of the resource group to be created.</span></span>
   * <span data-ttu-id="5d1c6-129">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-129">**-l (or --location)**.</span></span> <span data-ttu-id="5d1c6-130">Oblast Azure, kde se skupina prostředků vytvoří.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-130">Azure region where the resource group will be created.</span></span>
   * <span data-ttu-id="5d1c6-131">**-f (nebo --template-file)**.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-131">**-f (or --template-file)**.</span></span> <span data-ttu-id="5d1c6-132">Cesta k souboru šablony ARM.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-132">Path to your ARM template file.</span></span>
   * <span data-ttu-id="5d1c6-133">**-e (nebo --parameters-file)**.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-133">**-e (or --parameters-file)**.</span></span> <span data-ttu-id="5d1c6-134">Cesta k souboru parametrů ARM.</span><span class="sxs-lookup"><span data-stu-id="5d1c6-134">Path to your ARM parameters file.</span></span>

