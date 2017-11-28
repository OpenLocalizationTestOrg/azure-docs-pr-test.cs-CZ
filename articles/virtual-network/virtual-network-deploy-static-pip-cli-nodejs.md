---
title: "aaaCreate virtuální počítač s statickou veřejnou IP adresu - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální počítač s statickou veřejnou IP adresu pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
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
ms.openlocfilehash: 3ee906b65735830757b455df00f9f8d4373be3dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-cli-10"></a><span data-ttu-id="55eaa-103">Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="55eaa-103">Create a VM with a static public IP address using hello Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="55eaa-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="55eaa-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="55eaa-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="55eaa-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="55eaa-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="55eaa-106">Azure CLI 2.0</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="55eaa-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="55eaa-107">Azure CLI 1.0</span></span>](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [<span data-ttu-id="55eaa-108">Šablona</span><span class="sxs-lookup"><span data-stu-id="55eaa-108">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="55eaa-109">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="55eaa-109">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="55eaa-110">Azure nabízí dva různé modely nasazení pro vytváření a práci s prostředky: [nástroj Resource Manager a klasický režim](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="55eaa-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="55eaa-111">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="55eaa-111">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

<span data-ttu-id="55eaa-112">Můžete dokončit tuto úlohu pomocí hello 1.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="55eaa-112">You can complete this task using hello Azure CLI 1.0 (this article) or hello [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span></span> 

## <span data-ttu-id="55eaa-113"><a name = "create"></a>Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="55eaa-113"><a name = "create"></a>Step 1 - Start your script</span></span>
<span data-ttu-id="55eaa-114">Si můžete stáhnout skript úplné bash hello používá [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="55eaa-114">You can download hello full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-cli.sh).</span></span> <span data-ttu-id="55eaa-115">Proveďte následující kroky toochange hello skriptu toowork ve vašem prostředí hello:</span><span class="sxs-lookup"><span data-stu-id="55eaa-115">Complete hello following steps toochange hello script toowork in your environment:</span></span>

<span data-ttu-id="55eaa-116">Změna hodnoty hello hello proměnných níže na základě hodnot hello chcete toouse pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="55eaa-116">Change hello values of hello variables below based on hello values you want toouse for your deployment.</span></span> <span data-ttu-id="55eaa-117">Hello následující scénář toohello mapy hodnoty používané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="55eaa-117">hello following values map toohello scenario used in this article:</span></span>

```azurecli
# Set variables for hello new resource group
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

## <a name="step-2---create-hello-necessary-resources-for-your-vm"></a><span data-ttu-id="55eaa-118">Krok 2 – Vytvoření hello potřebné prostředky pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="55eaa-118">Step 2 - Create hello necessary resources for your VM</span></span>
<span data-ttu-id="55eaa-119">Před vytvořením virtuálního počítače, musíte skupinu prostředků, virtuální síť, veřejné IP adresy a síťovou kartu toobe používané hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="55eaa-119">Before creating a VM, you need a resource group, VNet, public IP, and NIC toobe used by hello VM.</span></span>

1. <span data-ttu-id="55eaa-120">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="55eaa-120">Create a new resource group.</span></span>

    ```azurecli
    azure group create $rgName $location
    ```

2. <span data-ttu-id="55eaa-121">Vytvoření hello virtuálních sítí a podsítí.</span><span class="sxs-lookup"><span data-stu-id="55eaa-121">Create hello VNet and subnet.</span></span>

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

3. <span data-ttu-id="55eaa-122">Vytvořte prostředek veřejné IP hello.</span><span class="sxs-lookup"><span data-stu-id="55eaa-122">Create hello public IP resource.</span></span>

    ```azurecli
    azure network public-ip create --resource-group $rgName \
        --name $pipName \
        --location $location \
        --allocation-method Static \
        --domain-name-label $dnsName
    ```

4. <span data-ttu-id="55eaa-123">Vytvořte pro hello virtuálního počítače v podsíti hello vytvořili výše, s veřejnou IP adresu hello hello síťové rozhraní (NIC).</span><span class="sxs-lookup"><span data-stu-id="55eaa-123">Create hello network interface (NIC) for hello VM in hello subnet created above, with hello public IP.</span></span> <span data-ttu-id="55eaa-124">Všimněte si hello první sadu příkazů jsou použité tooretrieve hello **Id** hello podsítě vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="55eaa-124">Notice hello first set of commands are used tooretrieve hello **Id** of hello subnet created above.</span></span>

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
   > <span data-ttu-id="55eaa-125">První příkaz výše používá Hello [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) a [manipulace s řetězci](http://tldp.org/LDP/abs/html/string-manipulation.html) (přesněji řečeno, odebrání dílčí řetězec).</span><span class="sxs-lookup"><span data-stu-id="55eaa-125">hello first command above uses [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) and [string manipulation](http://tldp.org/LDP/abs/html/string-manipulation.html) (more specifically, substring removal).</span></span>
   >

5. <span data-ttu-id="55eaa-126">Vytvořte hello toohost účet úložiště jednotky operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="55eaa-126">Create a storage account toohost hello VM OS drive.</span></span>

    ```azurecli
    azure storage account create $stdStorageAccountName \
        --resource-group $rgName \
        --location $location --type LRS
    ```

## <a name="step-3---create-hello-vm"></a><span data-ttu-id="55eaa-127">Krok 3 – vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="55eaa-127">Step 3 - Create hello VM</span></span>
<span data-ttu-id="55eaa-128">Teď, když všechny potřebné prostředky jsou na místě, můžete vytvořit nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="55eaa-128">Now that all necessary resources are in place, you can create a new VM.</span></span>

1. <span data-ttu-id="55eaa-129">Vytvořte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="55eaa-129">Create hello VM.</span></span>

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
2. <span data-ttu-id="55eaa-130">Uložte soubor skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="55eaa-130">Save hello script file.</span></span>

## <a name="step-4---run-hello-script"></a><span data-ttu-id="55eaa-131">Krok 4 – spustit skript hello</span><span class="sxs-lookup"><span data-stu-id="55eaa-131">Step 4 - Run hello script</span></span>
<span data-ttu-id="55eaa-132">Po provedení všechny potřebné změny a seznámit se s hello skriptu zobrazit výše, spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="55eaa-132">After making any necessary changes, and understanding hello script show above, run hello script.</span></span>

1. <span data-ttu-id="55eaa-133">Z konzoly bash spusťte skript hello výše.</span><span class="sxs-lookup"><span data-stu-id="55eaa-133">From a bash console, run hello script above.</span></span>

    ```azurecli
    sh myscript.sh
    ```

2. <span data-ttu-id="55eaa-134">Po několika minutách by měl zobrazit výstup Hello níže.</span><span class="sxs-lookup"><span data-stu-id="55eaa-134">hello output below should be displayed after a few minutes.</span></span>

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
        info:    Looking up hello subnet "FrontEnd"
        info:    Creating subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK
        info:    Executing command network public-ip create
        info:    Looking up hello public ip "PIPWEB1"
        info:    Creating public ip address "PIPWEB1"
        info:    Looking up hello public ip "PIPWEB1"
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
        info:    Looking up hello network interface "NICWEB1"
        info:    Looking up hello public ip "PIPWEB1"
        info:    Creating network interface "NICWEB1"
        info:    Looking up hello network interface "NICWEB1"
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
        info:    Looking up hello VM "WEB1"
        info:    Using hello VM Size "Standard_A1"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        info:    Looking up hello storage account iaasstorystorage
        info:    Looking up hello NIC "NICWEB1"
        info:    Creating VM "WEB1"
        info:    vm create command OK
