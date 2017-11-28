---
title: "aaaCreate virtuální počítač (klasický) s více síťovými kartami - prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální počítač (klasický) s více síťovými kartami pomocí prostředí PowerShell."
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
ms.openlocfilehash: 90c967929bb418042c3fb7079e0f69246faac53c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a><span data-ttu-id="8158d-103">Vytvoření virtuálního počítače (klasické) s více síťovými kartami pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8158d-103">Create a VM (Classic) with multiple NICs using PowerShell</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="8158d-104">Můžete vytvořit virtuální počítače (VM) v Azure a připojit více síťových rozhraní (NIC) tooeach vaše virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8158d-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="8158d-105">Několik síťových adaptérů povolit oddělení typů přenosů mezi síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="8158d-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="8158d-106">Například jedna síťová karta může komunikovat s hello Internetu, zatímco jiné komunikuje jenom s interním prostředkům není připojen toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="8158d-106">For example, one NIC might communicate with hello Internet, while another communicates only with internal resources not connected toohello Internet.</span></span> <span data-ttu-id="8158d-107">Hello možnost tooseparate síťový provoz na několik síťových adaptérů je vyžadována pro mnoho síťových virtuálních zařízení, například doručení aplikace a řešení optimalizace sítě WAN.</span><span class="sxs-lookup"><span data-stu-id="8158d-107">hello ability tooseparate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8158d-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="8158d-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="8158d-109">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="8158d-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="8158d-110">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="8158d-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="8158d-111">Zjistěte, jak tooperform tyto kroky, pomocí hello [modelu nasazení Resource Manager](virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="8158d-111">Learn how tooperform these steps using hello [Resource Manager deployment model](virtual-network-deploy-multinic-arm-ps.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="8158d-112">Hello následující postup použijte skupinu prostředků s názvem *IaaSStory* pro hello webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery hello DB.</span><span class="sxs-lookup"><span data-stu-id="8158d-112">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8158d-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8158d-113">Prerequisites</span></span>

<span data-ttu-id="8158d-114">Před vytvořením hello servery DB, musíte toocreate hello *IaaSStory* skupina prostředků se všechny hello potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="8158d-114">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="8158d-115">toocreate těchto prostředků, dokončení hello kroky, které následují.</span><span class="sxs-lookup"><span data-stu-id="8158d-115">toocreate these resources, complete hello steps that follow.</span></span> <span data-ttu-id="8158d-116">Vytvoření virtuální sítě pomocí následujících kroků hello v hello [vytvořit virtuální síť](virtual-networks-create-vnet-classic-netcfg-ps.md) článku.</span><span class="sxs-lookup"><span data-stu-id="8158d-116">Create a virtual network by following hello steps in hello [Create a virtual network](virtual-networks-create-vnet-classic-netcfg-ps.md) article.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="8158d-117">Vytvořit hello back-end virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8158d-117">Create hello back-end VMs</span></span>
<span data-ttu-id="8158d-118">Hello virtuálních počítačů v back-end závisí na vytvoření hello hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="8158d-118">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="8158d-119">**Back-end podsítě**.</span><span class="sxs-lookup"><span data-stu-id="8158d-119">**Backend subnet**.</span></span> <span data-ttu-id="8158d-120">Hello databázové servery budou součástí samostatnou podsíť, toosegregate provoz.</span><span class="sxs-lookup"><span data-stu-id="8158d-120">hello database servers will be part of a separate subnet, toosegregate traffic.</span></span> <span data-ttu-id="8158d-121">níže uvedený skript Hello očekává, že tuto podsíť tooexist ve virtuální síti s názvem *WTestVnet*.</span><span class="sxs-lookup"><span data-stu-id="8158d-121">hello script below expects this subnet tooexist in a vnet named *WTestVnet*.</span></span>
* <span data-ttu-id="8158d-122">**Účet úložiště pro datové disky**.</span><span class="sxs-lookup"><span data-stu-id="8158d-122">**Storage account for data disks**.</span></span> <span data-ttu-id="8158d-123">Pro lepší výkon použije hello datových disků na serverech databáze hello technologii SSD jednotky (SSD Solid-State Drive), která vyžaduje účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="8158d-123">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="8158d-124">Ujistěte se, zda text hello umístění Azure nasazujete toosupport storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="8158d-124">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="8158d-125">**Skupina dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="8158d-125">**Availability set**.</span></span> <span data-ttu-id="8158d-126">Všechny databázové servery budou přidány sady dostupnosti. jeden tooa, tooensure alespoň jeden z virtuálních počítačů hello je v provozu během údržby.</span><span class="sxs-lookup"><span data-stu-id="8158d-126">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="8158d-127">Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="8158d-127">Step 1 - Start your script</span></span>
<span data-ttu-id="8158d-128">Si můžete stáhnout hello úplné použít skript prostředí PowerShell [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="8158d-128">You can download hello full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span></span> <span data-ttu-id="8158d-129">Postupujte podle kroků hello toochange hello skriptu toowork ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="8158d-129">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="8158d-130">Změnit hello hodnoty proměnných hello níže podle vaší existující skupinu prostředků, které jsou nasazené výše v [požadavky](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="8158d-130">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. <span data-ttu-id="8158d-131">Změna hodnoty hello hello proměnných níže na základě hodnot hello chcete toouse pro vaše nasazení back-end.</span><span class="sxs-lookup"><span data-stu-id="8158d-131">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

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

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="8158d-132">Krok 2 – Vytvoření potřebné prostředky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8158d-132">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="8158d-133">Je nutné toocreate nová Cloudová služba a úložiště účtu pro hello datových disků pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8158d-133">You need toocreate a new cloud service and a storage account for hello data disks for all VMs.</span></span> <span data-ttu-id="8158d-134">Budete také potřebovat toospecify bitovou kopii a účet místního správce pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8158d-134">You also need toospecify an image, and a local administrator account for hello VMs.</span></span> <span data-ttu-id="8158d-135">dokončení těchto prostředků, toocreate hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8158d-135">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="8158d-136">Vytvořte novou cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="8158d-136">Create a new cloud service.</span></span>

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. <span data-ttu-id="8158d-137">Vytvořte nový účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="8158d-137">Create a new premium storage account.</span></span>

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. <span data-ttu-id="8158d-138">Účet úložiště hello sadu vytvořili výše jako hello aktuální účet úložiště pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="8158d-138">Set hello storage account created above as hello current storage account for your subscription.</span></span>

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. <span data-ttu-id="8158d-139">Vyberte bitovou kopii pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8158d-139">Select an image for hello VM.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. <span data-ttu-id="8158d-140">Nastavte přihlašovací údaje účtu místního správce hello.</span><span class="sxs-lookup"><span data-stu-id="8158d-140">Set hello local administrator account credentials.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a><span data-ttu-id="8158d-141">Krok 3 – vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8158d-141">Step 3 - Create VMs</span></span>
<span data-ttu-id="8158d-142">Musíte toouse toocreate smyčky, jak velký počet virtuálních počítačů a vytvořit hello potřebné síťové karty a virtuální počítače v rámci hello smyčky.</span><span class="sxs-lookup"><span data-stu-id="8158d-142">You need toouse a loop toocreate as many VMs as you want, and create hello necessary NICs and VMs within hello loop.</span></span> <span data-ttu-id="8158d-143">toocreate hello síťové karty a virtuální počítače, spusťte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="8158d-143">toocreate hello NICs and VMs, execute hello following steps.</span></span>

1. <span data-ttu-id="8158d-144">Spuštění `for` smyčky toorepeat hello příkazy toocreate virtuální počítač a dva síťové adaptéry s funkcí jako mnohokrát podle potřeby podle hello hodnotu hello `$numberOfVMs` proměnné.</span><span class="sxs-lookup"><span data-stu-id="8158d-144">Start a `for` loop toorepeat hello commands toocreate a VM and two NICs as many times as necessary, based on hello value of hello `$numberOfVMs` variable.</span></span>

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="8158d-145">Vytvoření `VMConfig` objekt hello obrázku, velikost a sadu dostupnosti pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8158d-145">Create a `VMConfig` object specifying hello image, size, and availability set for hello VM.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. <span data-ttu-id="8158d-146">Hello zřídit virtuální počítač jako virtuální počítač s Windows.</span><span class="sxs-lookup"><span data-stu-id="8158d-146">Provision hello VM as a Windows VM.</span></span>

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. <span data-ttu-id="8158d-147">Nastavit výchozí hello síťový adaptér a přiřaďte mu statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8158d-147">Set hello default NIC and assign it a static IP address.</span></span>

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. <span data-ttu-id="8158d-148">Přidejte druhý síťový adaptér pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8158d-148">Add a second NIC for each VM.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```

6. <span data-ttu-id="8158d-149">Vytvořte toodata disků pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8158d-149">Create toodata disks for each VM.</span></span>

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

7. <span data-ttu-id="8158d-150">Vytvořte každý virtuální počítač a end hello smyčky.</span><span class="sxs-lookup"><span data-stu-id="8158d-150">Create each VM, and end hello loop.</span></span>

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="8158d-151">Krok 4 – spustit skript hello</span><span class="sxs-lookup"><span data-stu-id="8158d-151">Step 4 - Run hello script</span></span>
<span data-ttu-id="8158d-152">Teď, když jste stáhli a změnit hello skriptu na základě potřeb, o mu skriptu toocreate hello back-end databáze virtuálních počítačů s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="8158d-152">Now that you downloaded and changed hello script based on your needs, runt he script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="8158d-153">Uložte skript a spusťte jej z hello **prostředí PowerShell** příkazového řádku, nebo **prostředí PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="8158d-153">Save your script and run it from hello **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="8158d-154">Zobrazí se výstup hello počáteční, a jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="8158d-154">You will see hello initial output, as shown below.</span></span>

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. <span data-ttu-id="8158d-155">Vyplňte hello informace potřebné v řádku hello přihlašovací údaje a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8158d-155">Fill out hello information needed in hello credentials prompt and click **OK**.</span></span> <span data-ttu-id="8158d-156">výstup Hello níže se vrátí.</span><span class="sxs-lookup"><span data-stu-id="8158d-156">hello output below is returned.</span></span>

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
