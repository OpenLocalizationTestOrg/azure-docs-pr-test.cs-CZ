---
title: "Vytvoření virtuálního počítače se statickou veřejnou IP adresu - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač se statickou veřejnou IP adresu pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a373c32271096308678fe3402e8420cc14fe5935
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli-10"></a><span data-ttu-id="f33ec-103">Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f33ec-103">Create a VM with a static public IP address using the Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f33ec-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f33ec-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="f33ec-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f33ec-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="f33ec-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f33ec-106">Azure CLI 2.0</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="f33ec-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f33ec-107">Azure CLI 1.0</span></span>](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [<span data-ttu-id="f33ec-108">Šablona</span><span class="sxs-lookup"><span data-stu-id="f33ec-108">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="f33ec-109">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="f33ec-109">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="f33ec-110">Azure nabízí dva různé modely nasazení pro vytváření a práci s prostředky: [nástroj Resource Manager a klasický režim](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f33ec-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f33ec-111">Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="f33ec-111">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

<span data-ttu-id="f33ec-112">Vám může tuto úlohu dokončit pomocí Azure CLI 1.0 (v tomto článku) nebo [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f33ec-112">You can complete this task using the Azure CLI 1.0 (this article) or the [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span></span> 

## <span data-ttu-id="f33ec-113"><a name = "create"></a>Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="f33ec-113"><a name = "create"></a>Step 1 - Start your script</span></span>
<span data-ttu-id="f33ec-114">Si můžete stáhnout skript úplné bash používá [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="f33ec-114">You can download the full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-cli.sh).</span></span> <span data-ttu-id="f33ec-115">Proveďte následující kroky, chcete-li změnit skript pro práci ve vašem prostředí:</span><span class="sxs-lookup"><span data-stu-id="f33ec-115">Complete the following steps to change the script to work in your environment:</span></span>

<span data-ttu-id="f33ec-116">Změňte hodnoty proměnných níže na základě hodnot, které chcete použít pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="f33ec-116">Change the values of the variables below based on the values you want to use for your deployment.</span></span> <span data-ttu-id="f33ec-117">Následující hodnoty mapování pro scénář používané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="f33ec-117">The following values map to the scenario used in this article:</span></span>

```azurecli
# Set variables for the new resource group
rgName="IaaSStory"
location="westus"

# Set variables for VNet
vnetName="TestVNet"
vnetPrefix="192.168.0.0/16"
subnetName="FrontEnd"
subnetPrefix="192.168.1.0/24"

# Set variables for storage
stdStorageAccountName="iaasstorystorage"

# Set variables for VM
vmSize="Standard_A1"
diskSize=127
publisher="Canonical"
offer="UbuntuServer"
sku="14.04.2-LTS"
version="latest"
vmName="WEB1"
osDiskName="osdisk"
nicName="NICWEB1"
privateIPAddress="192.168.1.101"
username='adminuser'
password='adminP@ssw0rd'
pipName="PIPWEB1"
dnsName="iaasstoryws1"
```

## <a name="step-2---create-the-necessary-resources-for-your-vm"></a><span data-ttu-id="f33ec-118">Krok 2 – Vytvoření nezbytné prostředky pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="f33ec-118">Step 2 - Create the necessary resources for your VM</span></span>
<span data-ttu-id="f33ec-119">Před vytvořením virtuálního počítače, musíte skupinu prostředků, virtuální sítě, veřejnou IP adresu a síťový adaptér, má být použit ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="f33ec-119">Before creating a VM, you need a resource group, VNet, public IP, and NIC to be used by the VM.</span></span>

1. <span data-ttu-id="f33ec-120">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f33ec-120">Create a new resource group.</span></span>

    ```azurecli
    azure group create $rgName $location
    ```

2. <span data-ttu-id="f33ec-121">Vytvořte virtuální síť a podsíť.</span><span class="sxs-lookup"><span data-stu-id="f33ec-121">Create the VNet and subnet.</span></span>

    ```azurecli
    azure network vnet create --resource-group $rgName \
        --name $vnetName \
        --address-prefixes $vnetPrefix \
        --location $location
    azure network vnet subnet create --resource-group $rgName \
        --vnet-name $vnetName \
        --name $subnetName \
        --address-prefix $subnetPrefix
    ```

3. <span data-ttu-id="f33ec-122">Vytvořte prostředek veřejné IP.</span><span class="sxs-lookup"><span data-stu-id="f33ec-122">Create the public IP resource.</span></span>

    ```azurecli
    azure network public-ip create --resource-group $rgName \
        --name $pipName \
        --location $location \
        --allocation-method Static \
        --domain-name-label $dnsName
    ```

4. <span data-ttu-id="f33ec-123">Vytvořte rozhraní sítě (NIC) pro virtuální počítač v podsíti vytvořili výše, s veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="f33ec-123">Create the network interface (NIC) for the VM in the subnet created above, with the public IP.</span></span> <span data-ttu-id="f33ec-124">Všimněte si první sadu příkazů slouží k načtení **Id** podsítě vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="f33ec-124">Notice the first set of commands are used to retrieve the **Id** of the subnet created above.</span></span>

    ```azurecli
    subnetId="$(azure network vnet subnet show --resource-group $rgName \
        --vnet-name $vnetName \
        --name $subnetName|grep Id)"

    subnetId=${subnetId#*/}

    azure network nic create --name $nicName \
        --resource-group $rgName \
        --location $location \
        --private-ip-address $privateIPAddress \
        --subnet-id $subnetId \
        --public-ip-name $pipName
    ```

   > [!TIP]
   > <span data-ttu-id="f33ec-125">První příkaz výše používá [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) a [manipulace s řetězci](http://tldp.org/LDP/abs/html/string-manipulation.html) (přesněji řečeno, odebrání dílčí řetězec).</span><span class="sxs-lookup"><span data-stu-id="f33ec-125">The first command above uses [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) and [string manipulation](http://tldp.org/LDP/abs/html/string-manipulation.html) (more specifically, substring removal).</span></span>
   >

5. <span data-ttu-id="f33ec-126">Vytvořte účet úložiště pro hostování jednotku operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f33ec-126">Create a storage account to host the VM OS drive.</span></span>

    ```azurecli
    azure storage account create $stdStorageAccountName \
        --resource-group $rgName \
        --location $location --type LRS
    ```

## <a name="step-3---create-the-vm"></a><span data-ttu-id="f33ec-127">Krok 3 – vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f33ec-127">Step 3 - Create the VM</span></span>
<span data-ttu-id="f33ec-128">Teď, když všechny potřebné prostředky jsou na místě, můžete vytvořit nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f33ec-128">Now that all necessary resources are in place, you can create a new VM.</span></span>

1. <span data-ttu-id="f33ec-129">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f33ec-129">Create the VM.</span></span>

    ```azurecli
    azure vm create --resource-group $rgName \
        --name $vmName \
        --location $location \
        --vm-size $vmSize \
        --subnet-id $subnetId \
        --nic-names $nicName \
        --os-type linux \
        --image-urn $publisher:$offer:$sku:$version \
        --storage-account-name $stdStorageAccountName \
        --storage-account-container-name vhds \
        --os-disk-vhd $osDiskName.vhd \
        --admin-username $username \
        --admin-password $password
    ```
2. <span data-ttu-id="f33ec-130">Uložte soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="f33ec-130">Save the script file.</span></span>

## <a name="step-4---run-the-script"></a><span data-ttu-id="f33ec-131">Krok 4 – spuštění skriptu</span><span class="sxs-lookup"><span data-stu-id="f33ec-131">Step 4 - Run the script</span></span>
<span data-ttu-id="f33ec-132">Po provedení všechny potřebné změny a seznámit se s skript zobrazení výše, spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="f33ec-132">After making any necessary changes, and understanding the script show above, run the script.</span></span>

1. <span data-ttu-id="f33ec-133">Z konzoly bash spusťte skript výše.</span><span class="sxs-lookup"><span data-stu-id="f33ec-133">From a bash console, run the script above.</span></span>

    ```azurecli
    sh myscript.sh
    ```

2. <span data-ttu-id="f33ec-134">Následující výstup má být zobrazena po několika minutách.</span><span class="sxs-lookup"><span data-stu-id="f33ec-134">The output below should be displayed after a few minutes.</span></span>

        info:    Executing command group create
        info:    Getting resource group IaaSStory
        info:    Creating resource group IaaSStory
        info:    Created resource group IaaSStory
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/IaaSStory
        data:    Name:                IaaSStory
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command network vnet create
        info:    Looking up virtual network "TestVNet"
        info:    Creating virtual network "TestVNet"
        info:    Loading virtual network state
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : westus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK
        info:    Executing command network vnet subnet create
        info:    Looking up the subnet "FrontEnd"
        info:    Creating subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK
        info:    Executing command network public-ip create
        info:    Looking up the public ip "PIPWEB1"
        info:    Creating public ip address "PIPWEB1"
        info:    Looking up the public ip "PIPWEB1"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/publicIPAddresses/PIPWEB1
        data:    Name                            : PIPWEB1
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Static
        data:    Idle timeout                    : 4
        data:    IP Address                      : 40.78.63.253
        data:    Domain name label               : iaasstoryws1
        data:    FQDN                            : iaasstoryws1.westus.cloudapp.azure.com
        info:    network public-ip create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICWEB1"
        info:    Looking up the public ip "PIPWEB1"
        info:    Creating network interface "NICWEB1"
        info:    Looking up the network interface "NICWEB1"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkInterfaces/NICWEB1
        data:    Name                            : NICWEB1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/publicIPAddresses/PIPWEB1
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory2/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up the VM "WEB1"
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account iaasstorystorage
        info:    Looking up the NIC "NICWEB1"
        info:    Creating VM "WEB1"
        info:    vm create command OK
