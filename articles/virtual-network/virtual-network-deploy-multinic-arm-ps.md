---
title: "Vytvoření virtuálního počítače s více síťovými kartami - prostředí Azure PowerShell | Microsoft Docs"
description: "Postup vytvoření virtuálního počítače s více síťovými kartami pomocí prostředí PowerShell."
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
ms.openlocfilehash: f3a11afd8fbd6a5e6b94cf1ebee7ea20665421bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-multiple-nics-using-powershell"></a><span data-ttu-id="5e679-103">Vytvoření virtuálního počítače s více síťovými kartami pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e679-103">Create a VM with multiple NICs using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e679-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e679-104">PowerShell</span></span>](virtual-network-deploy-multinic-arm-ps.md)
> * [<span data-ttu-id="5e679-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5e679-105">Azure CLI</span></span>](virtual-network-deploy-multinic-arm-cli.md)
> * [<span data-ttu-id="5e679-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="5e679-106">Template</span></span>](virtual-network-deploy-multinic-arm-template.md)
> * [<span data-ttu-id="5e679-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="5e679-107">PowerShell (Classic)</span></span>](virtual-network-deploy-multinic-classic-ps.md)
> * [<span data-ttu-id="5e679-108">Rozhraní příkazového řádku Azure (klasický)</span><span class="sxs-lookup"><span data-stu-id="5e679-108">Azure CLI (Classic)</span></span>](virtual-network-deploy-multinic-classic-cli.md)

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="5e679-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5e679-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="5e679-110">Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5e679-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="5e679-111">Následující postup použijte skupinu prostředků s názvem *IaaSStory* pro webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery DB.</span><span class="sxs-lookup"><span data-stu-id="5e679-111">The following steps use a resource group named *IaaSStory* for the WEB servers and a resource group named *IaaSStory-BackEnd* for the DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e679-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5e679-112">Prerequisites</span></span>
<span data-ttu-id="5e679-113">Před vytvořením servery DB, je potřeba vytvořit *IaaSStory* skupina prostředků se všechny potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="5e679-113">Before you can create the DB servers, you need to create the *IaaSStory* resource group with all the necessary resources for this scenario.</span></span> <span data-ttu-id="5e679-114">Pokud chcete vytvořit tyto prostředky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5e679-114">To create these resources, complete the following steps:</span></span>

1. <span data-ttu-id="5e679-115">Přejděte na [na stránku šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="5e679-115">Navigate to [the template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="5e679-116">Na stránce šablony napravo od **nadřazené skupiny prostředků**, klikněte na tlačítko **nasadit do Azure**.</span><span class="sxs-lookup"><span data-stu-id="5e679-116">In the template page, to the right of **Parent resource group**, click **Deploy to Azure**.</span></span>
3. <span data-ttu-id="5e679-117">V případě potřeby změňte hodnoty parametrů a potom postupujte podle kroků v portálu Azure preview nasazení skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5e679-117">If needed, change the parameter values, then follow the steps in the Azure preview portal to deploy the resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e679-118">Ujistěte se, že vaše názvy účtů úložiště jsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="5e679-118">Make sure your storage account names are unique.</span></span> <span data-ttu-id="5e679-119">Názvy účtů úložiště duplicitní nemůže mít v Azure.</span><span class="sxs-lookup"><span data-stu-id="5e679-119">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a><span data-ttu-id="5e679-120">Vytvořit virtuální počítače back-end</span><span class="sxs-lookup"><span data-stu-id="5e679-120">Create the back-end VMs</span></span>
<span data-ttu-id="5e679-121">Virtuální počítače back-end závisí na vytvoření v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="5e679-121">The back-end VMs depend on the creation of the following resources:</span></span>

* <span data-ttu-id="5e679-122">**Účet úložiště pro datové disky**.</span><span class="sxs-lookup"><span data-stu-id="5e679-122">**Storage account for data disks**.</span></span> <span data-ttu-id="5e679-123">Pro lepší výkon datové disky v databázových serverech použije technologii SSD jednotky (SSD Solid-State Drive), která vyžaduje účet úložiště premium.</span><span class="sxs-lookup"><span data-stu-id="5e679-123">For better performance, the data disks on the database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="5e679-124">Zajistěte, aby umístění Azure, můžete nasadit pro podporu služby storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="5e679-124">Make sure the Azure location you deploy to support premium storage.</span></span>
* <span data-ttu-id="5e679-125">**Síťové adaptéry**.</span><span class="sxs-lookup"><span data-stu-id="5e679-125">**NICs**.</span></span> <span data-ttu-id="5e679-126">Každý virtuální počítač bude mít dva síťové adaptéry, jeden pro přístup k databázi a jeden pro správu.</span><span class="sxs-lookup"><span data-stu-id="5e679-126">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="5e679-127">**Skupina dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="5e679-127">**Availability set**.</span></span> <span data-ttu-id="5e679-128">Všechny databázové servery se zařadí do jedné dostupnosti nastavte, ujistěte se, že nejméně jedna z virtuálních počítačů je nahoru a během údržby.</span><span class="sxs-lookup"><span data-stu-id="5e679-128">All database servers will be added to a single availability set, to ensure at least one of the VMs is up and running during maintenance.</span></span>  

### <a name="step-1---start-your-script"></a><span data-ttu-id="5e679-129">Krok 1 – spustit skript</span><span class="sxs-lookup"><span data-stu-id="5e679-129">Step 1 - Start your script</span></span>
<span data-ttu-id="5e679-130">Si můžete stáhnout úplné skript prostředí PowerShell použít [zde](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="5e679-130">You can download the full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span></span> <span data-ttu-id="5e679-131">Postupujte podle pokynů níže změňte skript pro práci ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="5e679-131">Follow the steps below to change the script to work in your environment.</span></span>

1. <span data-ttu-id="5e679-132">Změňte hodnoty proměnných níže podle vaší existující skupinu prostředků, které jsou nasazené výše v [požadavky](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="5e679-132">Change the values of the variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $existingRGName        = "IaaSStory"
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    $remoteAccessNSGName   = "NSG-RemoteAccess"
    $stdStorageAccountName = "wtestvnetstoragestd"
    ```

2. <span data-ttu-id="5e679-133">Změňte hodnoty proměnných níže na základě hodnot, které chcete použít pro vaše nasazení back-end.</span><span class="sxs-lookup"><span data-stu-id="5e679-133">Change the values of the variables below based on the values you want to use for your backend deployment.</span></span>

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
3. <span data-ttu-id="5e679-134">Načíst existující prostředky potřebné pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="5e679-134">Retrieve the existing resources needed for your deployment.</span></span>

    ```powershell
    $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
    $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
    $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
    $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="5e679-135">Krok 2 – Vytvoření potřebné prostředky pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="5e679-135">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="5e679-136">Potřebujete vytvořit novou skupinu prostředků, účet úložiště pro datové disky a sadu dostupnosti pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5e679-136">You need to create a new resource group, a storage account for the data disks, and an availability set for all VMs.</span></span> <span data-ttu-id="5e679-137">Můžete také potřebovat přihlašovací údaje účtu místního správce pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5e679-137">You alos need the local administrator account credentials for each VM.</span></span> <span data-ttu-id="5e679-138">Chcete-li vytvořit těchto prostředků, spusťte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="5e679-138">To create these resources, execute the following steps.</span></span>

1. <span data-ttu-id="5e679-139">Vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5e679-139">Create a new resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name $backendRGName -Location $location
    ```
2. <span data-ttu-id="5e679-140">Vytvořte nový účet úložiště premium ve skupině prostředků vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="5e679-140">Create a new premium storage account in the resource group created above.</span></span>

    ```powershell
    $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
    -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location
    ```
3. <span data-ttu-id="5e679-141">Vytvořte novou skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="5e679-141">Create a new availability set.</span></span>

    ```powershell
    $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location
    ```
4. <span data-ttu-id="5e679-142">Získáte přihlašovací údaje účtu, který se má použít pro každý virtuální počítač místního správce.</span><span class="sxs-lookup"><span data-stu-id="5e679-142">Get the local administrator account credentials to be used for each VM.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Type the name and password for the local administrator account."
    ```

### <a name="step-3---create-the-nics-and-back-end-vms"></a><span data-ttu-id="5e679-143">Krok 3 – Vytvoření síťové karty a virtuální počítače back-end</span><span class="sxs-lookup"><span data-stu-id="5e679-143">Step 3 - Create the NICs and back-end VMs</span></span>
<span data-ttu-id="5e679-144">Budete muset použít smyčku vytvořit libovolný počet virtuálních počítačů a vytvořit potřebné síťové karty a virtuální počítače v rámci smyčky.</span><span class="sxs-lookup"><span data-stu-id="5e679-144">You need to use a loop to create as many VMs as you want, and create the necessary NICs and VMs within the loop.</span></span> <span data-ttu-id="5e679-145">Chcete-li vytvořit síťové karty a virtuální počítače, spusťte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="5e679-145">To create the NICs and VMs, execute the following steps.</span></span>

1. <span data-ttu-id="5e679-146">Spuštění `for` cykly opakování příkazů pro vytvoření virtuálního počítače a dva síťové adaptéry tolikrát, kolikrát podle potřeby, na základě hodnoty z `$numberOfVMs` proměnné.</span><span class="sxs-lookup"><span data-stu-id="5e679-146">Start a `for` loop to repeat the commands to create a VM and two NICs as many times as necessary, based on the value of the `$numberOfVMs` variable.</span></span>
   
    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="5e679-147">Vytvořte síťová karta použitá pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="5e679-147">Create the NIC used for database access.</span></span>

    ```powershell
    $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
    $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
    $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1
    ```

3. <span data-ttu-id="5e679-148">Vytvořte na síťový adaptér použitý pro vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="5e679-148">Create the NIC used for remote access.</span></span> <span data-ttu-id="5e679-149">Všimněte si, jak má skupinu NSG přidruženou k němu tento síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="5e679-149">Notice how this NIC has an NSG associated to it.</span></span>

    ```powershell
    $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
    $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
    $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
    -NetworkSecurityGroupId $remoteAccessNSG.Id
    ```

4. <span data-ttu-id="5e679-150">Vytvoření `vmConfig` objektu.</span><span class="sxs-lookup"><span data-stu-id="5e679-150">Create `vmConfig` object.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
    ```

5. <span data-ttu-id="5e679-151">Vytvořte dvě datové disky na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5e679-151">Create two data disks per VM.</span></span> <span data-ttu-id="5e679-152">Všimněte si, zda jsou disky dat v účtu úložiště premium vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="5e679-152">Notice that the data disks are in the premium storage account created earlier.</span></span>

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

6. <span data-ttu-id="5e679-153">Konfigurace operačního systému a bitové kopie, který se má použít pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5e679-153">Configure the operating system, and image to be used for the VM.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version
    ```

7. <span data-ttu-id="5e679-154">Přidejte dva síťové adaptéry vytvořili výše `vmConfig` objektu.</span><span class="sxs-lookup"><span data-stu-id="5e679-154">Add the two NICs created above to the `vmConfig` object.</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id
    ```

8. <span data-ttu-id="5e679-155">Vytvořte disk operačního systému a vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5e679-155">Create the OS disk and create the VM.</span></span> <span data-ttu-id="5e679-156">Upozornění `}` končící `for` smyčky.</span><span class="sxs-lookup"><span data-stu-id="5e679-156">Notice the `}` ending the `for` loop.</span></span>

    ```powershell
    $osDiskName = $vmName + "-" + $osDiskSuffix
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
    }
    ```

### <a name="step-4---run-the-script"></a><span data-ttu-id="5e679-157">Krok 4 – spuštění skriptu</span><span class="sxs-lookup"><span data-stu-id="5e679-157">Step 4 - Run the script</span></span>
<span data-ttu-id="5e679-158">Teď, když jste stáhli a změnit skript na základě potřeb, o, které mu skript pro vytvoření back-end databáze virtuálních počítačů s více síťovými kartami.</span><span class="sxs-lookup"><span data-stu-id="5e679-158">Now that you downloaded and changed the script based on your needs, runt he script to create the back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="5e679-159">Uložte skript a spusťte jej z **prostředí PowerShell** příkazového řádku, nebo **prostředí PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="5e679-159">Save your script and run it from the **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="5e679-160">Zobrazí se počáteční výstup, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5e679-160">You will see the initial output, as follows:</span></span>

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
            Actions  NotActions
            =======  ==========
                *                  

        ResourceId        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend

2. <span data-ttu-id="5e679-161">Po několika minutách vyplnit řádku přihlašovací údaje a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e679-161">After a few minutes, fill out the credentials prompt and click **OK**.</span></span> <span data-ttu-id="5e679-162">Výstup níže představuje jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5e679-162">The output below represents a single VM.</span></span> <span data-ttu-id="5e679-163">Všimněte si celý proces trvalo 8 minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="5e679-163">Notice the entire process took 8 minutes to complete.</span></span>

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
