---
title: "aaaCreate virtuálního počítače s více síťovými kartami - prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuálního počítače s více síťovými kartami pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88880483-8f9e-4eeb-b783-64b8613407d9
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 507a413510da3ee69aefed324977ee40e442268b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-powershell"></a><span data-ttu-id="365ce-103">Vytvoření virtuálního počítače s více síťovými kartami pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="365ce-103">Create a VM with multiple NICs using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="365ce-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="365ce-104">PowerShell</span></span>](virtual-network-deploy-multinic-arm-ps.md)
> * [<span data-ttu-id="365ce-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="365ce-105">Azure CLI</span></span>](virtual-network-deploy-multinic-arm-cli.md)
> * [<span data-ttu-id="365ce-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="365ce-106">Template</span></span>](virtual-network-deploy-multinic-arm-template.md)
> * [<span data-ttu-id="365ce-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="365ce-107">PowerShell (Classic)</span></span>](virtual-network-deploy-multinic-classic-ps.md)
> * [<span data-ttu-id="365ce-108">Rozhraní příkazového řádku Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="365ce-108">Azure CLI (Classic)</span></span>](virtual-network-deploy-multinic-classic-cli.md)

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="365ce-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="365ce-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="365ce-110">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="365ce-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="365ce-111">Hello následující postup použijte skupinu prostředků s názvem *IaaSStory* pro hello webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery hello DB.</span><span class="sxs-lookup"><span data-stu-id="365ce-111">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="365ce-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="365ce-112">Prerequisites</span></span>
<span data-ttu-id="365ce-113">Před vytvořením hello servery DB, musíte toocreate hello *IaaSStory* skupina prostředků se všechny hello potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="365ce-113">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="365ce-114">dokončení těchto prostředků, toocreate hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="365ce-114">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="365ce-115">Přejděte příliš[stránku hello šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="365ce-115">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="365ce-116">V stránku hello šablony, toohello napravo od **nadřazené skupiny prostředků**, klikněte na tlačítko **nasazení tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="365ce-116">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="365ce-117">V případě potřeby změňte hodnoty parametrů hello a potom postupujte podle kroků hello ve skupině prostředků hello toodeploy portálu Azure preview hello.</span><span class="sxs-lookup"><span data-stu-id="365ce-117">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="365ce-118">Ujistěte se, že vaše názvy účtů úložiště jsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="365ce-118">Make sure your storage account names are unique.</span></span> <span data-ttu-id="365ce-119">Názvy účtů úložiště duplicitní nemůže mít v Azure.</span><span class="sxs-lookup"><span data-stu-id="365ce-119">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="365ce-120">Vytvořit hello back-end virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="365ce-120">Create hello back-end VMs</span></span>
<span data-ttu-id="365ce-121">Hello virtuálních počítačů v back-end závisí na vytvoření hello hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="365ce-121">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="365ce-122">**Účet úložiště pro datové disky**.</span><span class="sxs-lookup"><span data-stu-id="365ce-122">**Storage account for data disks**.</span></span> <span data-ttu-id="365ce-123">Pro lepší výkon použije hello datových disků na serverech databáze hello technologii SSD jednotky (SSD Solid-State Drive), která vyžaduje účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="365ce-123">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="365ce-124">Ujistěte se, zda text hello umístění Azure nasazujete toosupport storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="365ce-124">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="365ce-125">**Síťové adaptéry**.</span><span class="sxs-lookup"><span data-stu-id="365ce-125">**NICs**.</span></span> <span data-ttu-id="365ce-126">Každý virtuální počítač bude mít dva síťové adaptéry, jeden pro přístup k databázi a jeden pro správu.</span><span class="sxs-lookup"><span data-stu-id="365ce-126">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="365ce-127">**Skupina dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="365ce-127">**Availability set**.</span></span> <span data-ttu-id="365ce-128">Všechny databázové servery budou přidány sady dostupnosti. jeden tooa, tooensure alespoň jeden z virtuálních počítačů hello je v provozu během údržby.</span><span class="sxs-lookup"><span data-stu-id="365ce-128">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>  

### <a name="step-1---start-your-script"></a><span data-ttu-id="365ce-129">Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="365ce-129">Step 1 - Start your script</span></span>
<span data-ttu-id="365ce-130">Si můžete stáhnout hello úplné použít skript prostředí PowerShell [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="365ce-130">You can download hello full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span></span> <span data-ttu-id="365ce-131">Postupujte podle kroků hello toochange hello skriptu toowork ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="365ce-131">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="365ce-132">Změnit hello hodnoty proměnných hello níže podle vaší existující skupinu prostředků, které jsou nasazené výše v [požadavky](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="365ce-132">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $existingRGName        = "IaaSStory"
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    $remoteAccessNSGName   = "NSG-RemoteAccess"
    $stdStorageAccountName = "wtestvnetstoragestd"
    ```

2. <span data-ttu-id="365ce-133">Změna hodnoty hello hello proměnných níže na základě hodnot hello chcete toouse pro vaše nasazení back-end.</span><span class="sxs-lookup"><span data-stu-id="365ce-133">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```powershell
    $backendRGName         = "IaaSStory-Backend"
    $prmStorageAccountName = "wtestvnetstorageprm"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $publisher             = "MicrosoftSQLServer"
    $offer                 = "SQL2014SP1-WS2012R2"
    $sku                   = "Standard"
    $version               = "latest"
    $vmNamePrefix          = "DB"
    $osDiskPrefix          = "osdiskdb"
    $dataDiskPrefix        = "datadisk"
    $diskSize               = "120"    
    $nicNamePrefix         = "NICDB"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```
3. <span data-ttu-id="365ce-134">Načíst hello existující prostředky potřebné pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="365ce-134">Retrieve hello existing resources needed for your deployment.</span></span>

    ```powershell
    $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
    $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
    $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
    $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="365ce-135">Krok 2 – Vytvoření potřebné prostředky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="365ce-135">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="365ce-136">Potřebujete toocreate novou skupinu prostředků, účet úložiště pro hello datových disků, a sadu dostupnosti pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="365ce-136">You need toocreate a new resource group, a storage account for hello data disks, and an availability set for all VMs.</span></span> <span data-ttu-id="365ce-137">Také musíte hello přihlašovací údaje účtu místního správce pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="365ce-137">You alos need hello local administrator account credentials for each VM.</span></span> <span data-ttu-id="365ce-138">spuštění těchto prostředků, toocreate hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="365ce-138">toocreate these resources, execute hello following steps.</span></span>

1. <span data-ttu-id="365ce-139">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="365ce-139">Create a new resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name $backendRGName -Location $location
    ```
2. <span data-ttu-id="365ce-140">Vytvořte nový účet úložiště premium ve skupině prostředků hello vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="365ce-140">Create a new premium storage account in hello resource group created above.</span></span>

    ```powershell
    $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
    -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location
    ```
3. <span data-ttu-id="365ce-141">Vytvořte novou skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="365ce-141">Create a new availability set.</span></span>

    ```powershell
    $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location
    ```
4. <span data-ttu-id="365ce-142">Získáte oprávnění místního správce hello toobe přihlašovací údaje účtu použít pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="365ce-142">Get hello local administrator account credentials toobe used for each VM.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Type hello name and password for hello local administrator account."
    ```

### <a name="step-3---create-hello-nics-and-back-end-vms"></a><span data-ttu-id="365ce-143">Krok 3 – vytvoření hello síťové karty a virtuální počítače back-end</span><span class="sxs-lookup"><span data-stu-id="365ce-143">Step 3 - Create hello NICs and back-end VMs</span></span>
<span data-ttu-id="365ce-144">Musíte toouse toocreate smyčky, jak velký počet virtuálních počítačů a vytvořit hello potřebné síťové karty a virtuální počítače v rámci hello smyčky.</span><span class="sxs-lookup"><span data-stu-id="365ce-144">You need toouse a loop toocreate as many VMs as you want, and create hello necessary NICs and VMs within hello loop.</span></span> <span data-ttu-id="365ce-145">toocreate hello síťové karty a virtuální počítače, spusťte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="365ce-145">toocreate hello NICs and VMs, execute hello following steps.</span></span>

1. <span data-ttu-id="365ce-146">Spuštění `for` smyčky toorepeat hello příkazy toocreate virtuální počítač a dva síťové adaptéry s funkcí jako mnohokrát podle potřeby podle hello hodnotu hello `$numberOfVMs` proměnné.</span><span class="sxs-lookup"><span data-stu-id="365ce-146">Start a `for` loop toorepeat hello commands toocreate a VM and two NICs as many times as necessary, based on hello value of hello `$numberOfVMs` variable.</span></span>
   
    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="365ce-147">Vytvořte hello síťový adaptér používá pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="365ce-147">Create hello NIC used for database access.</span></span>

    ```powershell
    $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
    $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
    $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1
    ```

3. <span data-ttu-id="365ce-148">Vytvořte hello síťový adaptér použitý pro vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="365ce-148">Create hello NIC used for remote access.</span></span> <span data-ttu-id="365ce-149">Všimněte si, jak má tento síťový adaptér tooit NSG přidruženou.</span><span class="sxs-lookup"><span data-stu-id="365ce-149">Notice how this NIC has an NSG associated tooit.</span></span>

    ```powershell
    $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
    $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
    $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
    -NetworkSecurityGroupId $remoteAccessNSG.Id
    ```

4. <span data-ttu-id="365ce-150">Vytvoření `vmConfig` objektu.</span><span class="sxs-lookup"><span data-stu-id="365ce-150">Create `vmConfig` object.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
    ```

5. <span data-ttu-id="365ce-151">Vytvořte dvě datové disky na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="365ce-151">Create two data disks per VM.</span></span> <span data-ttu-id="365ce-152">Všimněte si, zda jsou disky hello dat v účtu úložiště premium hello vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="365ce-152">Notice that hello data disks are in hello premium storage account created earlier.</span></span>

    ```powershell
    $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"
    $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
    Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
    -VhdUri $data1VhdUri -CreateOption empty -Lun 0

    $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"
    $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
    Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
    -VhdUri $data2VhdUri -CreateOption empty -Lun 1
    ```

6. <span data-ttu-id="365ce-153">Nakonfigurujte hello operačního systému a bitové kopie toobe používá pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="365ce-153">Configure hello operating system, and image toobe used for hello VM.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version
    ```

7. <span data-ttu-id="365ce-154">Přidání síťových adaptérů hello dva vytvořili výše toohello `vmConfig` objektu.</span><span class="sxs-lookup"><span data-stu-id="365ce-154">Add hello two NICs created above toohello `vmConfig` object.</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id
    ```

8. <span data-ttu-id="365ce-155">Vytvořte disk s operačním systémem hello a vytvořte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="365ce-155">Create hello OS disk and create hello VM.</span></span> <span data-ttu-id="365ce-156">Všimněte si hello `}` ukončení hello `for` smyčky.</span><span class="sxs-lookup"><span data-stu-id="365ce-156">Notice hello `}` ending hello `for` loop.</span></span>

    ```powershell
    $osDiskName = $vmName + "-" + $osDiskSuffix
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
    }
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="365ce-157">Krok 4 – spustit skript hello</span><span class="sxs-lookup"><span data-stu-id="365ce-157">Step 4 - Run hello script</span></span>
<span data-ttu-id="365ce-158">Teď, když jste stáhli a změnit hello skriptu na základě potřeb, o mu skriptu toocreate hello back-end databáze virtuálních počítačů s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="365ce-158">Now that you downloaded and changed hello script based on your needs, runt he script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="365ce-159">Uložte skript a spusťte jej z hello **prostředí PowerShell** příkazového řádku, nebo **prostředí PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="365ce-159">Save your script and run it from hello **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="365ce-160">Zobrazí se výstup počáteční hello, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="365ce-160">You will see hello initial output, as follows:</span></span>

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
            Actions  NotActions
            =======  ==========
                *                  

        ResourceId        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend

2. <span data-ttu-id="365ce-161">Po několika minutách vyplnit řádku hello přihlašovací údaje a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="365ce-161">After a few minutes, fill out hello credentials prompt and click **OK**.</span></span> <span data-ttu-id="365ce-162">výstup Hello níže představuje jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="365ce-162">hello output below represents a single VM.</span></span> <span data-ttu-id="365ce-163">Všimněte si hello celý proces trvalo toocomplete po 8 minutách.</span><span class="sxs-lookup"><span data-stu-id="365ce-163">Notice hello entire process took 8 minutes toocomplete.</span></span>

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText :  {
                                    "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Compute/availabilitySets/ASDB"
                                    }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                        "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0
        EndTime             : [Date] [Time]
        Error               :
        Output              :
        StartTime           : [Date] [Time]
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
