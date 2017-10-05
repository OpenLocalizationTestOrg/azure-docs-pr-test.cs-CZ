---
title: "Vytvoření virtuálního počítače (klasické) s více síťovými kartami - prostředí Azure PowerShell | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač (klasický) s více síťovými kartami pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6e50f39a-2497-4845-a5d4-7332dbc203c5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 923d4817d96399fc423b0a89cbf88f8d397f1af0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a><span data-ttu-id="199b0-103">Vytvoření virtuálního počítače (klasické) s více síťovými kartami pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="199b0-103">Create a VM (Classic) with multiple NICs using PowerShell</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="199b0-104">Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) ke každému z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="199b0-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) to each of your VMs.</span></span> <span data-ttu-id="199b0-105">Několik síťových adaptérů povolit oddělení typů přenosů mezi síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="199b0-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="199b0-106">Jeden síťový adaptér může například komunikují přes Internet, zatímco jiné komunikuje jenom s interním prostředkům, které nejsou připojené k Internetu.</span><span class="sxs-lookup"><span data-stu-id="199b0-106">For example, one NIC might communicate with the Internet, while another communicates only with internal resources not connected to the Internet.</span></span> <span data-ttu-id="199b0-107">Schopnost oddělit síťový provoz na několik síťových adaptérů je vyžadována pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN.</span><span class="sxs-lookup"><span data-stu-id="199b0-107">The ability to separate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="199b0-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="199b0-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="199b0-109">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="199b0-109">This article covers using the classic deployment model.</span></span> <span data-ttu-id="199b0-110">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="199b0-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="199b0-111">Naučte se provádět tyto kroky, pomocí [modelu nasazení Resource Manager](virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="199b0-111">Learn how to perform these steps using the [Resource Manager deployment model](virtual-network-deploy-multinic-arm-ps.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="199b0-112">Následující postup použijte skupinu prostředků s názvem *IaaSStory* pro webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery DB.</span><span class="sxs-lookup"><span data-stu-id="199b0-112">The following steps use a resource group named *IaaSStory* for the WEB servers and a resource group named *IaaSStory-BackEnd* for the DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="199b0-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="199b0-113">Prerequisites</span></span>

<span data-ttu-id="199b0-114">Před vytvořením servery DB, je potřeba vytvořit *IaaSStory* skupina prostředků se všechny potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="199b0-114">Before you can create the DB servers, you need to create the *IaaSStory* resource group with all the necessary resources for this scenario.</span></span> <span data-ttu-id="199b0-115">Chcete-li vytvořit tyto prostředky, proveďte kroky, které následují.</span><span class="sxs-lookup"><span data-stu-id="199b0-115">To create these resources, complete the steps that follow.</span></span> <span data-ttu-id="199b0-116">Vytvoření virtuální sítě pomocí následujících kroků v [vytvořit virtuální síť](virtual-networks-create-vnet-classic-netcfg-ps.md) článku.</span><span class="sxs-lookup"><span data-stu-id="199b0-116">Create a virtual network by following the steps in the [Create a virtual network](virtual-networks-create-vnet-classic-netcfg-ps.md) article.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a><span data-ttu-id="199b0-117">Vytvořit virtuální počítače back-end</span><span class="sxs-lookup"><span data-stu-id="199b0-117">Create the back-end VMs</span></span>
<span data-ttu-id="199b0-118">Virtuální počítače back-end závisí na vytvoření v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="199b0-118">The back-end VMs depend on the creation of the following resources:</span></span>

* <span data-ttu-id="199b0-119">**Back-end podsítě**.</span><span class="sxs-lookup"><span data-stu-id="199b0-119">**Backend subnet**.</span></span> <span data-ttu-id="199b0-120">Databázové servery budou součástí samostatnou podsíť, oddělit provoz.</span><span class="sxs-lookup"><span data-stu-id="199b0-120">The database servers will be part of a separate subnet, to segregate traffic.</span></span> <span data-ttu-id="199b0-121">Níže uvedený skript předpokládá, že tuto podsíť existovat ve virtuální síti s názvem *WTestVnet*.</span><span class="sxs-lookup"><span data-stu-id="199b0-121">The script below expects this subnet to exist in a vnet named *WTestVnet*.</span></span>
* <span data-ttu-id="199b0-122">**Účet úložiště pro datové disky**.</span><span class="sxs-lookup"><span data-stu-id="199b0-122">**Storage account for data disks**.</span></span> <span data-ttu-id="199b0-123">Pro lepší výkon datové disky v databázových serverech použije technologii SSD jednotky (SSD Solid-State Drive), která vyžaduje účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="199b0-123">For better performance, the data disks on the database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="199b0-124">Zajistěte, aby umístění Azure, můžete nasadit pro podporu služby storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="199b0-124">Make sure the Azure location you deploy to support premium storage.</span></span>
* <span data-ttu-id="199b0-125">**Skupina dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="199b0-125">**Availability set**.</span></span> <span data-ttu-id="199b0-126">Všechny databázové servery se zařadí do jedné dostupnosti nastavte, ujistěte se, že nejméně jedna z virtuálních počítačů je nahoru a během údržby.</span><span class="sxs-lookup"><span data-stu-id="199b0-126">All database servers will be added to a single availability set, to ensure at least one of the VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="199b0-127">Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="199b0-127">Step 1 - Start your script</span></span>
<span data-ttu-id="199b0-128">Si můžete stáhnout úplné skript prostředí PowerShell použít [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="199b0-128">You can download the full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span></span> <span data-ttu-id="199b0-129">Postupujte podle pokynů níže změňte skript pro práci ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="199b0-129">Follow the steps below to change the script to work in your environment.</span></span>

1. <span data-ttu-id="199b0-130">Změňte hodnoty proměnných níže podle vaší existující skupinu prostředků, které jsou nasazené výše v [požadavky](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="199b0-130">Change the values of the variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. <span data-ttu-id="199b0-131">Změňte hodnoty proměnných níže na základě hodnot, které chcete použít pro vaše nasazení back-end.</span><span class="sxs-lookup"><span data-stu-id="199b0-131">Change the values of the variables below based on the values you want to use for your backend deployment.</span></span>

    ```powershell
    $backendCSName         = "IaaSStory-Backend"
    $prmStorageAccountName = "iaasstoryprmstorage"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $diskSize              = 127
    $vmNamePrefix          = "DB"
    $dataDiskSuffix        = "datadisk"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="199b0-132">Krok 2 – Vytvoření potřebné prostředky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="199b0-132">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="199b0-133">Potřebujete vytvořit novou cloudovou službu a účet úložiště pro datové disky pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="199b0-133">You need to create a new cloud service and a storage account for the data disks for all VMs.</span></span> <span data-ttu-id="199b0-134">Také musíte zadat bitovou kopii a účet místního správce pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="199b0-134">You also need to specify an image, and a local administrator account for the VMs.</span></span> <span data-ttu-id="199b0-135">Pokud chcete vytvořit tyto prostředky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="199b0-135">To create these resources, complete the following steps:</span></span>

1. <span data-ttu-id="199b0-136">Vytvořte novou cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="199b0-136">Create a new cloud service.</span></span>

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. <span data-ttu-id="199b0-137">Vytvořte nový účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="199b0-137">Create a new premium storage account.</span></span>

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. <span data-ttu-id="199b0-138">Nastavte účet úložiště vytvořili výše jako aktuální účet úložiště pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="199b0-138">Set the storage account created above as the current storage account for your subscription.</span></span>

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. <span data-ttu-id="199b0-139">Vyberte bitovou kopii pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="199b0-139">Select an image for the VM.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. <span data-ttu-id="199b0-140">Nastavte přihlašovací údaje k účtu místního správce.</span><span class="sxs-lookup"><span data-stu-id="199b0-140">Set the local administrator account credentials.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a><span data-ttu-id="199b0-141">Krok 3 – vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="199b0-141">Step 3 - Create VMs</span></span>
<span data-ttu-id="199b0-142">Budete muset použít smyčku vytvořit libovolný počet virtuálních počítačů a vytvořit potřebné síťové karty a virtuální počítače v rámci smyčky.</span><span class="sxs-lookup"><span data-stu-id="199b0-142">You need to use a loop to create as many VMs as you want, and create the necessary NICs and VMs within the loop.</span></span> <span data-ttu-id="199b0-143">Chcete-li vytvořit síťové karty a virtuální počítače, spusťte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="199b0-143">To create the NICs and VMs, execute the following steps.</span></span>

1. <span data-ttu-id="199b0-144">Spuštění `for` cykly opakování příkazů pro vytvoření virtuálního počítače a dva síťové adaptéry tolikrát, kolikrát podle potřeby, na základě hodnoty z `$numberOfVMs` proměnné.</span><span class="sxs-lookup"><span data-stu-id="199b0-144">Start a `for` loop to repeat the commands to create a VM and two NICs as many times as necessary, based on the value of the `$numberOfVMs` variable.</span></span>

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="199b0-145">Vytvoření `VMConfig` objekt obrázku, velikost a sadu dostupnosti pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="199b0-145">Create a `VMConfig` object specifying the image, size, and availability set for the VM.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. <span data-ttu-id="199b0-146">Zřídit virtuální počítač jako virtuální počítač systému Windows.</span><span class="sxs-lookup"><span data-stu-id="199b0-146">Provision the VM as a Windows VM.</span></span>

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. <span data-ttu-id="199b0-147">Nastavit výchozí síťový adaptér a přiřaďte mu statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="199b0-147">Set the default NIC and assign it a static IP address.</span></span>

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. <span data-ttu-id="199b0-148">Přidejte druhý síťový adaptér pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="199b0-148">Add a second NIC for each VM.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```

6. <span data-ttu-id="199b0-149">Vytvoření datových disků, pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="199b0-149">Create to data disks for each VM.</span></span>

    ```powershell
    $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk1Name `
    -LUN 0

    $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk2Name `
    -LUN 1
    ```

7. <span data-ttu-id="199b0-150">Vytvořit jednotlivé virtuální počítače a ukončení smyčky.</span><span class="sxs-lookup"><span data-stu-id="199b0-150">Create each VM, and end the loop.</span></span>

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-the-script"></a><span data-ttu-id="199b0-151">Krok 4 – spuštění skriptu</span><span class="sxs-lookup"><span data-stu-id="199b0-151">Step 4 - Run the script</span></span>
<span data-ttu-id="199b0-152">Teď, když jste stáhli a změnit skript na základě potřeb, o, které mu skript pro vytvoření back-end databáze virtuálních počítačů s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="199b0-152">Now that you downloaded and changed the script based on your needs, runt he script to create the back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="199b0-153">Uložte skript a spusťte jej z **prostředí PowerShell** příkazového řádku, nebo **prostředí PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="199b0-153">Save your script and run it from the **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="199b0-154">Počáteční výstupu uvidíte, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="199b0-154">You will see the initial output, as shown below.</span></span>

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. <span data-ttu-id="199b0-155">Vyplňte informace potřebné v řádku přihlašovací údaje a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="199b0-155">Fill out the information needed in the credentials prompt and click **OK**.</span></span> <span data-ttu-id="199b0-156">Následující výstup se vrací.</span><span class="sxs-lookup"><span data-stu-id="199b0-156">The output below is returned.</span></span>

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
