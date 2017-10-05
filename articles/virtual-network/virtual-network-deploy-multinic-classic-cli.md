---
title: "Vytvoření virtuálního počítače (klasické) s více síťovými kartami - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač (klasický) s více síťovými kartami pomocí rozhraní příkazového řádku Azure (CLI) 1.0."
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
ms.openlocfilehash: b62421b7289650818748d0016dccfdf42ef0a768
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-the-azure-cli-10"></a><span data-ttu-id="cd693-103">Vytvoření virtuálního počítače (klasické) s více síťovými kartami pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="cd693-103">Create a VM (Classic) with multiple NICs using the Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="cd693-104">Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) ke každému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cd693-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) to each of your VMs.</span></span> <span data-ttu-id="cd693-105">Několik síťových adaptérů povolit oddělení typů přenosů mezi síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="cd693-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="cd693-106">Jeden síťový adaptér může například komunikují přes Internet, zatímco jiné komunikuje jenom s interním prostředkům, které nejsou připojené k Internetu.</span><span class="sxs-lookup"><span data-stu-id="cd693-106">For example, one NIC might communicate with the Internet, while another communicates only with internal resources not connected to the Internet.</span></span> <span data-ttu-id="cd693-107">Schopnost oddělit síťový provoz na několik síťových adaptérů je vyžadována pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN.</span><span class="sxs-lookup"><span data-stu-id="cd693-107">The ability to separate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd693-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="cd693-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="cd693-109">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="cd693-109">This article covers using the classic deployment model.</span></span> <span data-ttu-id="cd693-110">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cd693-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="cd693-111">Naučte se provádět tyto kroky, pomocí [modelu nasazení Resource Manager](virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="cd693-111">Learn how to perform these steps using the [Resource Manager deployment model](virtual-network-deploy-multinic-arm-cli.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="cd693-112">Následující postup použijte skupinu prostředků s názvem *IaaSStory* pro webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery DB.</span><span class="sxs-lookup"><span data-stu-id="cd693-112">The following steps use a resource group named *IaaSStory* for the WEB servers and a resource group named *IaaSStory-BackEnd* for the DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd693-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd693-113">Prerequisites</span></span>
<span data-ttu-id="cd693-114">Před vytvořením servery DB, je potřeba vytvořit *IaaSStory* skupina prostředků se všechny potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="cd693-114">Before you can create the DB servers, you need to create the *IaaSStory* resource group with all the necessary resources for this scenario.</span></span> <span data-ttu-id="cd693-115">Chcete-li vytvořit tyto prostředky, proveďte kroky, které následují.</span><span class="sxs-lookup"><span data-stu-id="cd693-115">To create these resources, complete the steps that follow.</span></span> <span data-ttu-id="cd693-116">Vytvoření virtuální sítě pomocí následujících kroků v [vytvořit virtuální síť](virtual-networks-create-vnet-classic-cli.md) článku.</span><span class="sxs-lookup"><span data-stu-id="cd693-116">Create a virtual network by following the steps in the [Create a virtual network](virtual-networks-create-vnet-classic-cli.md) article.</span></span>

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a><span data-ttu-id="cd693-117">Nasazení virtuálních počítačů back-end</span><span class="sxs-lookup"><span data-stu-id="cd693-117">Deploy the back-end VMs</span></span>
<span data-ttu-id="cd693-118">Virtuální počítače back-end závisí na vytvoření v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="cd693-118">The back-end VMs depend on the creation of the following resources:</span></span>

* <span data-ttu-id="cd693-119">**Účet úložiště pro datové disky**.</span><span class="sxs-lookup"><span data-stu-id="cd693-119">**Storage account for data disks**.</span></span> <span data-ttu-id="cd693-120">Pro lepší výkon datové disky v databázových serverech použije technologii SSD jednotky (SSD Solid-State Drive), která vyžaduje účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="cd693-120">For better performance, the data disks on the database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="cd693-121">Zajistěte, aby umístění Azure, můžete nasadit pro podporu služby storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="cd693-121">Make sure the Azure location you deploy to support premium storage.</span></span>
* <span data-ttu-id="cd693-122">**Síťové adaptéry**.</span><span class="sxs-lookup"><span data-stu-id="cd693-122">**NICs**.</span></span> <span data-ttu-id="cd693-123">Každý virtuální počítač bude mít dva síťové adaptéry, jeden pro přístup k databázi a jeden pro správu.</span><span class="sxs-lookup"><span data-stu-id="cd693-123">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="cd693-124">**Skupina dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="cd693-124">**Availability set**.</span></span> <span data-ttu-id="cd693-125">Všechny databázové servery se zařadí do jedné dostupnosti nastavte, ujistěte se, že nejméně jedna z virtuálních počítačů je nahoru a během údržby.</span><span class="sxs-lookup"><span data-stu-id="cd693-125">All database servers will be added to a single availability set, to ensure at least one of the VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="cd693-126">Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="cd693-126">Step 1 - Start your script</span></span>
<span data-ttu-id="cd693-127">Si můžete stáhnout skript úplné bash používá [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="cd693-127">You can download the full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh).</span></span> <span data-ttu-id="cd693-128">Proveďte následující kroky, chcete-li změnit skript pro práci ve vašem prostředí:</span><span class="sxs-lookup"><span data-stu-id="cd693-128">Complete the following steps to change the script to work in your environment:</span></span>

1. <span data-ttu-id="cd693-129">Změňte hodnoty proměnných níže podle vaší existující skupinu prostředků, které jsou nasazené výše v [požadavky](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="cd693-129">Change the values of the variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```azurecli
    location="useast2"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    ```
2. <span data-ttu-id="cd693-130">Změňte hodnoty proměnných níže na základě hodnot, které chcete použít pro vaše nasazení back-end.</span><span class="sxs-lookup"><span data-stu-id="cd693-130">Change the values of the variables below based on the values you want to use for your backend deployment.</span></span>

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

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="cd693-131">Krok 2 – Vytvoření potřebné prostředky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="cd693-131">Step 2 - Create necessary resources for your VMs</span></span>
1. <span data-ttu-id="cd693-132">Vytvořte novou cloudovou službu pro všechny virtuální počítače back-end.</span><span class="sxs-lookup"><span data-stu-id="cd693-132">Create a new cloud service for all backend VMs.</span></span> <span data-ttu-id="cd693-133">Všimněte si použití `$backendCSName` proměnná pro název skupiny prostředků a `$location` pro oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="cd693-133">Notice the use of the `$backendCSName` variable for the resource group name, and `$location` for the Azure region.</span></span>

    ```azurecli
    azure service create --serviceName $backendCSName \
        --location $location
    ```

2. <span data-ttu-id="cd693-134">Vytvořte účet úložiště premium pro disky operačního systému a dat má být používána vaše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="cd693-134">Create a premium storage account for the OS and data disks to be used by yours VMs.</span></span>

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --location $location \
        --type PLRS
    ```

### <a name="step-3---create-vms-with-multiple-nics"></a><span data-ttu-id="cd693-135">Krok 3 – vytvoření virtuálních počítačů s více síťovými kartami</span><span class="sxs-lookup"><span data-stu-id="cd693-135">Step 3 - Create VMs with multiple NICs</span></span>
1. <span data-ttu-id="cd693-136">Spustit smyčku vytvořit víc virtuálních počítačů, na základě `numberOfVMs` proměnné.</span><span class="sxs-lookup"><span data-stu-id="cd693-136">Start a loop to create multiple VMs, based on the `numberOfVMs` variables.</span></span>

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. <span data-ttu-id="cd693-137">Pro každý virtuální počítač zadejte název a IP adresu každé dva síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="cd693-137">For each VM, specify the name and IP address of each of the two NICs.</span></span>

    ```azurecli
    nic1Name=$vmNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x

    nic2Name=$vmNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    ```

3. <span data-ttu-id="cd693-138">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cd693-138">Create the VM.</span></span> <span data-ttu-id="cd693-139">Všimněte si použití `--nic-config` parametr, který obsahuje seznam všech síťových adaptérů s názvem, podsíť a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="cd693-139">Notice the usage of the `--nic-config` parameter, containing a list of all NICs with name, subnet, and IP address.</span></span>

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

4. <span data-ttu-id="cd693-140">Pro každý virtuální počítač vytvořte dva datových disků.</span><span class="sxs-lookup"><span data-stu-id="cd693-140">For each VM, create two data disks.</span></span>

    ```azurecli
    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
    done
    ```

### <a name="step-4---run-the-script"></a><span data-ttu-id="cd693-141">Krok 4 – spuštění skriptu</span><span class="sxs-lookup"><span data-stu-id="cd693-141">Step 4 - Run the script</span></span>
<span data-ttu-id="cd693-142">Teď, když jste stáhli a změnit na základě potřeb skriptu, spusťte skript pro vytvoření back-end databáze virtuálních počítačů s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="cd693-142">Now that you downloaded and changed the script based on your needs, run the script to create the back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="cd693-143">Uložte skript a spusťte jej z vaší **Bash** terminálu.</span><span class="sxs-lookup"><span data-stu-id="cd693-143">Save your script and run it from your **Bash** terminal.</span></span> <span data-ttu-id="cd693-144">Počáteční výstupu uvidíte, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="cd693-144">You will see the initial output, as shown below.</span></span>

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

2. <span data-ttu-id="cd693-145">Po několika minutách se ukončí provádění a zbytek výstupu se zobrazí, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="cd693-145">After a few minutes, the execution will end and you will see the rest of the output as shown below.</span></span>

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
