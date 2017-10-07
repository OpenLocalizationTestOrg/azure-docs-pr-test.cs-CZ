---
title: "aaaCreate virtuální počítač (klasický) s více síťovými kartami - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální počítač (klasický) s více síťovými kartami pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b436e41e-866c-439f-a7c7-7b4b041725ef
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 181bfb28027caff33410ca94744e79206a2a0d0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="c0286-103">Vytvoření virtuálního počítače (klasické) s více síťovými kartami pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c0286-103">Create a VM (Classic) with multiple NICs using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="c0286-104">Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) tooeach vaše virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c0286-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="c0286-105">Několik síťových adaptérů povolit oddělení typů přenosů mezi síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="c0286-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="c0286-106">Například jedna síťová karta může komunikovat s hello Internetu, zatímco jiné komunikuje jenom s interním prostředkům není připojen toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="c0286-106">For example, one NIC might communicate with hello Internet, while another communicates only with internal resources not connected toohello Internet.</span></span> <span data-ttu-id="c0286-107">Hello možnost tooseparate síťový provoz na několik síťových adaptérů je vyžadována pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN.</span><span class="sxs-lookup"><span data-stu-id="c0286-107">hello ability tooseparate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0286-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c0286-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c0286-109">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="c0286-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="c0286-110">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="c0286-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="c0286-111">Zjistěte, jak tooperform tyto kroky, pomocí hello [modelu nasazení Resource Manager](virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c0286-111">Learn how tooperform these steps using hello [Resource Manager deployment model](virtual-network-deploy-multinic-arm-cli.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="c0286-112">Hello následující postup použijte skupinu prostředků s názvem *IaaSStory* pro hello webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery hello DB.</span><span class="sxs-lookup"><span data-stu-id="c0286-112">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0286-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0286-113">Prerequisites</span></span>
<span data-ttu-id="c0286-114">Před vytvořením hello servery DB, musíte toocreate hello *IaaSStory* skupina prostředků se všechny hello potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="c0286-114">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="c0286-115">toocreate těchto prostředků, dokončení hello kroky, které následují.</span><span class="sxs-lookup"><span data-stu-id="c0286-115">toocreate these resources, complete hello steps that follow.</span></span> <span data-ttu-id="c0286-116">Vytvoření virtuální sítě pomocí následujících kroků hello v hello [vytvořit virtuální síť](virtual-networks-create-vnet-classic-cli.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c0286-116">Create a virtual network by following hello steps in hello [Create a virtual network](virtual-networks-create-vnet-classic-cli.md) article.</span></span>

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-hello-back-end-vms"></a><span data-ttu-id="c0286-117">Nasazení hello back-end virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c0286-117">Deploy hello back-end VMs</span></span>
<span data-ttu-id="c0286-118">Hello virtuálních počítačů v back-end závisí na vytvoření hello hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="c0286-118">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="c0286-119">**Účet úložiště pro datové disky**.</span><span class="sxs-lookup"><span data-stu-id="c0286-119">**Storage account for data disks**.</span></span> <span data-ttu-id="c0286-120">Pro lepší výkon použije hello datových disků na serverech databáze hello technologii SSD jednotky (SSD Solid-State Drive), která vyžaduje účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="c0286-120">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="c0286-121">Ujistěte se, zda text hello umístění Azure nasazujete toosupport storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="c0286-121">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="c0286-122">**Síťové adaptéry**.</span><span class="sxs-lookup"><span data-stu-id="c0286-122">**NICs**.</span></span> <span data-ttu-id="c0286-123">Každý virtuální počítač bude mít dva síťové adaptéry, jeden pro přístup k databázi a jeden pro správu.</span><span class="sxs-lookup"><span data-stu-id="c0286-123">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="c0286-124">**Skupina dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c0286-124">**Availability set**.</span></span> <span data-ttu-id="c0286-125">Všechny databázové servery budou přidány sady dostupnosti. jeden tooa, tooensure alespoň jeden z virtuálních počítačů hello je v provozu během údržby.</span><span class="sxs-lookup"><span data-stu-id="c0286-125">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="c0286-126">Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="c0286-126">Step 1 - Start your script</span></span>
<span data-ttu-id="c0286-127">Si můžete stáhnout skript úplné bash hello používá [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="c0286-127">You can download hello full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh).</span></span> <span data-ttu-id="c0286-128">Proveďte následující kroky toochange hello skriptu toowork ve vašem prostředí hello:</span><span class="sxs-lookup"><span data-stu-id="c0286-128">Complete hello following steps toochange hello script toowork in your environment:</span></span>

1. <span data-ttu-id="c0286-129">Změnit hello hodnoty proměnných hello níže podle vaší existující skupinu prostředků, které jsou nasazené výše v [požadavky](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="c0286-129">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```azurecli
    location="useast2"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    ```
2. <span data-ttu-id="c0286-130">Změna hodnoty hello hello proměnných níže na základě hodnot hello chcete toouse pro vaše nasazení back-end.</span><span class="sxs-lookup"><span data-stu-id="c0286-130">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```azurecli
    backendCSName="IaaSStory-Backend"
    prmStorageAccountName="iaasstoryprmstorage"
    image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskPrefix="db"
    dataDiskName="datadisk"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="c0286-131">Krok 2 – Vytvoření potřebné prostředky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c0286-131">Step 2 - Create necessary resources for your VMs</span></span>
1. <span data-ttu-id="c0286-132">Vytvořte novou cloudovou službu pro všechny virtuální počítače back-end.</span><span class="sxs-lookup"><span data-stu-id="c0286-132">Create a new cloud service for all backend VMs.</span></span> <span data-ttu-id="c0286-133">Všimněte si použití hello hello `$backendCSName` proměnná pro název skupiny prostředků hello, a `$location` pro hello oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="c0286-133">Notice hello use of hello `$backendCSName` variable for hello resource group name, and `$location` for hello Azure region.</span></span>

    ```azurecli
    azure service create --serviceName $backendCSName \
        --location $location
    ```

2. <span data-ttu-id="c0286-134">Vytvořte účet úložiště premium pro hello operačního systému a datové disky toobe používá vaše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c0286-134">Create a premium storage account for hello OS and data disks toobe used by yours VMs.</span></span>

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --location $location \
        --type PLRS
    ```

### <a name="step-3---create-vms-with-multiple-nics"></a><span data-ttu-id="c0286-135">Krok 3 – vytvoření virtuálních počítačů s více síťovými kartami</span><span class="sxs-lookup"><span data-stu-id="c0286-135">Step 3 - Create VMs with multiple NICs</span></span>
1. <span data-ttu-id="c0286-136">Spuštění několika virtuálních počítačů, podle hello smyčky toocreate `numberOfVMs` proměnné.</span><span class="sxs-lookup"><span data-stu-id="c0286-136">Start a loop toocreate multiple VMs, based on hello `numberOfVMs` variables.</span></span>

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. <span data-ttu-id="c0286-137">Pro každý virtuální počítač zadejte hello názvu a IP adresy každé hello dva síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="c0286-137">For each VM, specify hello name and IP address of each of hello two NICs.</span></span>

    ```azurecli
    nic1Name=$vmNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x

    nic2Name=$vmNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    ```

3. <span data-ttu-id="c0286-138">Vytvořte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c0286-138">Create hello VM.</span></span> <span data-ttu-id="c0286-139">Všimněte si použití hello hello `--nic-config` parametr, který obsahuje seznam všech síťových adaptérů s názvem, podsíť a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c0286-139">Notice hello usage of hello `--nic-config` parameter, containing a list of all NICs with name, subnet, and IP address.</span></span>

    ```azurecli
    azure vm create $backendCSName $image $username $password \
        --connect $backendCSName \
        --vm-name $vmNamePrefix$suffixNumber \
        --vm-size $vmSize \
        --availability-set $avSetName \
        --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
        --virtual-network-name $vnetName \
        --subnet-names $backendSubnetName \
        --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::
    ```

4. <span data-ttu-id="c0286-140">Pro každý virtuální počítač vytvořte dva datových disků.</span><span class="sxs-lookup"><span data-stu-id="c0286-140">For each VM, create two data disks.</span></span>

    ```azurecli
    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
    done
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="c0286-141">Krok 4 – spustit skript hello</span><span class="sxs-lookup"><span data-stu-id="c0286-141">Step 4 - Run hello script</span></span>
<span data-ttu-id="c0286-142">Teď, když jste stáhli a změnit podle svých potřeb, spusťte hello skriptu toocreate hello zpět skriptu hello ukončení databáze virtuálních počítačů s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="c0286-142">Now that you downloaded and changed hello script based on your needs, run hello script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="c0286-143">Uložte skript a spusťte jej z vaší **Bash** terminálu.</span><span class="sxs-lookup"><span data-stu-id="c0286-143">Save your script and run it from your **Bash** terminal.</span></span> <span data-ttu-id="c0286-144">Zobrazí se výstup hello počáteční, a jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c0286-144">You will see hello initial output, as shown below.</span></span>

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. <span data-ttu-id="c0286-145">Po několika minutách se ukončí provádění hello a zobrazí se hello zbytek hello výstup, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c0286-145">After a few minutes, hello execution will end and you will see hello rest of hello output as shown below.</span></span>

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
