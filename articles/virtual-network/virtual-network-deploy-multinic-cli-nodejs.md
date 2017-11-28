---
title: "aaaCreate virtuálního počítače s více síťovými kartami - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuálního počítače s více síťovými kartami pomocí Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 07c660b632bcdc004365a6f910ecf8a5c13cbc6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="c1d63-103">Vytvoření virtuálního počítače s více síťovými kartami pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c1d63-103">Create a VM with multiple NICs using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="c1d63-104">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c1d63-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="c1d63-105">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](virtual-network-deploy-multinic-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c1d63-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-cli.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="c1d63-106">Hello následující postup použijte skupinu prostředků s názvem *IaaSStory* pro hello webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery hello DB.</span><span class="sxs-lookup"><span data-stu-id="c1d63-106">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span> <span data-ttu-id="c1d63-107">Můžete dokončit tuto úlohu pomocí hello 1.0 rozhraní příkazového řádku Azure (v tomto článku) nebo hello [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c1d63-107">You can complete this task using hello Azure CLI 1.0 (this article) or hello [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span></span> <span data-ttu-id="c1d63-108">Hello hodnoty v "" pro proměnné hello hello kroky, které následují vytvořit prostředky s nastavením hello scénáře.</span><span class="sxs-lookup"><span data-stu-id="c1d63-108">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="c1d63-109">Změnit hello hodnoty, jako je vhodné pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1d63-109">Change hello values, as appropriate, for your environment.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1d63-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1d63-110">Prerequisites</span></span>
<span data-ttu-id="c1d63-111">Před vytvořením hello servery DB, musíte toocreate hello *IaaSStory* skupina prostředků se všechny hello potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="c1d63-111">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="c1d63-112">dokončení těchto prostředků, toocreate hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c1d63-112">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="c1d63-113">Přejděte příliš[stránku hello šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="c1d63-113">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="c1d63-114">V stránku hello šablony, toohello napravo od **nadřazené skupiny prostředků**, klikněte na tlačítko **nasazení tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="c1d63-114">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="c1d63-115">V případě potřeby změňte hodnoty parametrů hello a potom postupujte podle kroků hello ve skupině prostředků hello toodeploy portálu Azure preview hello.</span><span class="sxs-lookup"><span data-stu-id="c1d63-115">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c1d63-116">Ujistěte se, že vaše názvy účtů úložiště jsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="c1d63-116">Make sure your storage account names are unique.</span></span> <span data-ttu-id="c1d63-117">Názvy účtů úložiště duplicitní nemůže mít v Azure.</span><span class="sxs-lookup"><span data-stu-id="c1d63-117">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="c1d63-118">Vytvořit hello back-end virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c1d63-118">Create hello back-end VMs</span></span>
<span data-ttu-id="c1d63-119">Hello virtuálních počítačů v back-end závisí na vytvoření hello hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="c1d63-119">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="c1d63-120">**Účet úložiště pro datové disky**.</span><span class="sxs-lookup"><span data-stu-id="c1d63-120">**Storage account for data disks**.</span></span> <span data-ttu-id="c1d63-121">Pro lepší výkon použije hello datových disků na serverech databáze hello technologii SSD jednotky (SSD Solid-State Drive), která vyžaduje účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="c1d63-121">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="c1d63-122">Ujistěte se, zda text hello umístění Azure nasazujete toosupport storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="c1d63-122">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="c1d63-123">**Síťové adaptéry**.</span><span class="sxs-lookup"><span data-stu-id="c1d63-123">**NICs**.</span></span> <span data-ttu-id="c1d63-124">Každý virtuální počítač bude mít dva síťové adaptéry, jeden pro přístup k databázi a jeden pro správu.</span><span class="sxs-lookup"><span data-stu-id="c1d63-124">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="c1d63-125">**Skupina dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c1d63-125">**Availability set**.</span></span> <span data-ttu-id="c1d63-126">Všechny databázové servery budou přidány sady dostupnosti. jeden tooa, tooensure alespoň jeden z virtuálních počítačů hello je v provozu během údržby.</span><span class="sxs-lookup"><span data-stu-id="c1d63-126">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="c1d63-127">Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="c1d63-127">Step 1 - Start your script</span></span>
<span data-ttu-id="c1d63-128">Si můžete stáhnout skript úplné bash hello používá [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="c1d63-128">You can download hello full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh).</span></span> <span data-ttu-id="c1d63-129">Postupujte podle kroků hello toochange hello skriptu toowork ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1d63-129">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="c1d63-130">Změnit hello hodnoty proměnných hello níže podle vaší existující skupinu prostředků, které jsou nasazené výše v [požadavky](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="c1d63-130">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```azurecli
    existingRGName="IaaSStory"
    location="westus"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    remoteAccessNSGName="NSG-RemoteAccess"
    ```
2. <span data-ttu-id="c1d63-131">Změna hodnoty hello hello proměnných níže na základě hodnot hello chcete toouse pro vaše nasazení back-end.</span><span class="sxs-lookup"><span data-stu-id="c1d63-131">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```azurecli
    backendRGName="IaaSStory-Backend"
    prmStorageAccountName="wtestvnetstorageprm"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    publisher="Canonical"
    offer="UbuntuServer"
    sku="14.04.2-LTS"
    version="latest"
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskName="datadisk"
    nicNamePrefix="NICDB"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

3. <span data-ttu-id="c1d63-132">Načíst hello ID pro hello `BackEnd` podsíť, kde bude vytvořen hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c1d63-132">Retrieve hello ID for hello `BackEnd` subnet where hello VMs will be created.</span></span> <span data-ttu-id="c1d63-133">Tuto funkci potřebujete toodo vzhledem k tomu, že hello síťové adaptéry toobe přidružené toothis podsítě jsou v jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="c1d63-133">You need toodo this since hello NICs toobe associated toothis subnet are in a different resource group.</span></span>

    ```azurecli
    subnetId="$(azure network vnet subnet show --resource-group $existingRGName \
            --vnet-name $vnetName \
            --name $backendSubnetName|grep Id)"
    subnetId=${subnetId#*/}
    ```

   > [!TIP]
   > <span data-ttu-id="c1d63-134">První příkaz výše používá Hello [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) a [manipulace s řetězci](http://tldp.org/LDP/abs/html/string-manipulation.html) (přesněji řečeno, odebrání dílčí řetězec).</span><span class="sxs-lookup"><span data-stu-id="c1d63-134">hello first command above uses [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) and [string manipulation](http://tldp.org/LDP/abs/html/string-manipulation.html) (more specifically, substring removal).</span></span>
   >

4. <span data-ttu-id="c1d63-135">Načíst hello ID pro hello `NSG-RemoteAccess` NSG.</span><span class="sxs-lookup"><span data-stu-id="c1d63-135">Retrieve hello ID for hello `NSG-RemoteAccess` NSG.</span></span> <span data-ttu-id="c1d63-136">Tuto funkci potřebujete toodo vzhledem k tomu, že hello síťové adaptéry toobe přidružené toothis NSG jsou v jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="c1d63-136">You need toodo this since hello NICs toobe associated toothis NSG are in a different resource group.</span></span>

    ```azurecli
    nsgId="$(azure network nsg show --resource-group $existingRGName \
        --name $remoteAccessNSGName|grep Id)"
        nsgId=${nsgId#*/}
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="c1d63-137">Krok 2 – Vytvoření potřebné prostředky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c1d63-137">Step 2 - Create necessary resources for your VMs</span></span>

1. <span data-ttu-id="c1d63-138">Vytvořte novou skupinu prostředků pro všechny prostředky back-end.</span><span class="sxs-lookup"><span data-stu-id="c1d63-138">Create a new resource group for all backend resources.</span></span> <span data-ttu-id="c1d63-139">Všimněte si použití hello hello `$backendRGName` proměnná pro název skupiny prostředků hello, a `$location` pro hello oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="c1d63-139">Notice hello use of hello `$backendRGName` variable for hello resource group name, and `$location` for hello Azure region.</span></span>

    ```azurecli
    azure group create $backendRGName $location
    ```

2. <span data-ttu-id="c1d63-140">Vytvořte účet úložiště premium pro hello operačního systému a datové disky toobe používá vaše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c1d63-140">Create a premium storage account for hello OS and data disks toobe used by yours VMs.</span></span>

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --resource-group $backendRGName \
        --location $location \
        --type PLRS
    ```

3. <span data-ttu-id="c1d63-141">Vytvořte sadu dostupnosti pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c1d63-141">Create an availability set for hello VMs.</span></span>

    ```azurecli
    azure availset create --resource-group $backendRGName \
        --location $location \
        --name $avSetName
    ```

### <a name="step-3---create-hello-nics-and-back-end-vms"></a><span data-ttu-id="c1d63-142">Krok 3 – vytvoření hello síťové karty a virtuální počítače back-end</span><span class="sxs-lookup"><span data-stu-id="c1d63-142">Step 3 - Create hello NICs and back-end VMs</span></span>

1. <span data-ttu-id="c1d63-143">Spuštění několika virtuálních počítačů, podle hello smyčky toocreate `numberOfVMs` proměnné.</span><span class="sxs-lookup"><span data-stu-id="c1d63-143">Start a loop toocreate multiple VMs, based on hello `numberOfVMs` variables.</span></span>

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. <span data-ttu-id="c1d63-144">Pro každý virtuální počítač vytvořte síťovou kartu pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="c1d63-144">For each VM, create a NIC for database access.</span></span>

    ```azurecli
    nic1Name=$nicNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x
    azure network nic create --name $nic1Name \
        --resource-group $backendRGName \
        --location $location \
        --private-ip-address $ipAddress1 \
        --subnet-id $subnetId
    ```

3. <span data-ttu-id="c1d63-145">Pro každý virtuální počítač vytvořte síťovou kartu pro vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="c1d63-145">For each VM, create a NIC for remote access.</span></span> <span data-ttu-id="c1d63-146">Všimněte si hello `--network-security-group` parametr použité tooassociate hello seskupování tooan NSG.</span><span class="sxs-lookup"><span data-stu-id="c1d63-146">Notice hello `--network-security-group` parameter, used tooassociate hello NIC tooan NSG.</span></span>

    ```azurecli
    nic2Name=$nicNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    azure network nic create --name $nic2Name \
        --resource-group $backendRGName \
        --location $location \
        --private-ip-address $ipAddress2 \
        --subnet-id $subnetId $vnetName \
        --network-security-group-id $nsgId
    ```

4. <span data-ttu-id="c1d63-147">Vytvořte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c1d63-147">Create hello VM.</span></span>

    ```azurecli
    azure vm create --resource-group $backendRGName \
        --name $vmNamePrefix$suffixNumber \
        --location $location \
        --vm-size $vmSize \
        --subnet-id $subnetId \
        --availset-name $avSetName \
        --nic-names $nic1Name,$nic2Name \
        --os-type linux \
        --image-urn $publisher:$offer:$sku:$version \
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --os-disk-vhd $osDiskName$suffixNumber.vhd \
        --admin-username $username \
        --admin-password $password
    ```

5. <span data-ttu-id="c1d63-148">Pro každý virtuální počítač, vytvořte dvě datové disky a end hello smyčky s hello `done` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c1d63-148">For each VM, create two data disks, and end hello loop with hello `done` command.</span></span>

    ```azurecli
    azure vm disk attach-new --resource-group $backendRGName \
        --vm-name $vmNamePrefix$suffixNumber \
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --vhd-name $dataDiskName$suffixNumber-1.vhd \
        --size-in-gb $diskSize \
        --lun 0

    azure vm disk attach-new --resource-group $backendRGName \
        --vm-name $vmNamePrefix$suffixNumber \        
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --vhd-name $dataDiskName$suffixNumber-2.vhd \
        --size-in-gb $diskSize \
        --lun 1
        done
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="c1d63-149">Krok 4 – spustit skript hello</span><span class="sxs-lookup"><span data-stu-id="c1d63-149">Step 4 - Run hello script</span></span>
<span data-ttu-id="c1d63-150">Teď, když jste stáhli a změnit podle svých potřeb, spusťte hello skriptu toocreate hello zpět skriptu hello ukončení databáze virtuálních počítačů s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="c1d63-150">Now that you downloaded and changed hello script based on your needs, run hello script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="c1d63-151">Uložte skript a spusťte jej z vaší **Bash** terminálu.</span><span class="sxs-lookup"><span data-stu-id="c1d63-151">Save your script and run it from your **Bash** terminal.</span></span> <span data-ttu-id="c1d63-152">Zobrazí se výstup hello počáteční, a jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c1d63-152">You will see hello initial output, as shown below.</span></span>
   
        info:    Executing command group create
        info:    Getting resource group IaaSStory-Backend
        info:    Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command availset create
        info:    Looking up hello availability set "ASDB"
        info:    Creating availability set "ASDB"
        info:    availset create command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB1-DA"
        info:    Creating network interface "NICDB1-DA"
        info:    Looking up hello network interface "NICDB1-DA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-DA
        data:    Name                            : NICDB1-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB1-RA"
        info:    Creating network interface "NICDB1-RA"
        info:    Looking up hello network interface "NICDB1-RA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-RA
        data:    Name                            : NICDB1-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.54
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up hello VM "DB1"
        info:    Using hello VM Size "Standard_DS3"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    Looking up hello availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up hello NIC "NICDB1-DA"
        info:    Looking up hello NIC "NICDB1-RA"
        info:    Creating VM "DB1"
2. <span data-ttu-id="c1d63-153">Po několika minutách se ukončí provádění hello a zobrazí se hello zbytek hello výstup, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c1d63-153">After a few minutes, hello execution will end and you will see hello rest of hello output as shown below.</span></span>
   
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB1"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-1.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB1"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-2.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB2-DA"
        info:    Creating network interface "NICDB2-DA"
        info:    Looking up hello network interface "NICDB2-DA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-DA
        data:    Name                            : NICDB2-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.5
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB2-RA"
        info:    Creating network interface "NICDB2-RA"
        info:    Looking up hello network interface "NICDB2-RA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-RA
        data:    Name                            : NICDB2-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.55
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up hello VM "DB2"
        info:    Using hello VM Size "Standard_DS3"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    Looking up hello availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up hello NIC "NICDB2-DA"
        info:    Looking up hello NIC "NICDB2-RA"
        info:    Creating VM "DB2"
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB2"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-1.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB2"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-2.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK

