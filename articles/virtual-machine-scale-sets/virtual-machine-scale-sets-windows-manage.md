---
title: "Spravovat virtuální počítače ve Škálovací sadě virtuálních počítačů | Microsoft Docs"
description: "Správa virtuálních počítačů v škálování virtuálních počítačů, nastavit pomocí prostředí Azure PowerShell."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: d09a020b903e5f43afe03b86c675bcc1eb536cbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="430c9-103">Spravovat virtuální počítače ve škálovací sadě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="430c9-103">Manage virtual machines in a virtual machine scale set</span></span>
<span data-ttu-id="430c9-104">Úkoly v tomto článku použijte ke správě virtuálních počítačů sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="430c9-104">Use the tasks in this article to manage virtual machines in your virtual machine scale set.</span></span>

<span data-ttu-id="430c9-105">Většinu úloh, které zahrnují spravuje virtuální počítač ve škálovací sadě vyžadovat, že znáte ID instance na počítač, který chcete spravovat.</span><span class="sxs-lookup"><span data-stu-id="430c9-105">Most of the tasks that involve managing a virtual machine in a scale set require that you know the instance ID of the machine that you want to manage.</span></span> <span data-ttu-id="430c9-106">Můžete použít [Průzkumníka prostředků Azure](https://resources.azure.com) k vyhledání ID instance virtuálního počítače ve škálovací sadě.</span><span class="sxs-lookup"><span data-stu-id="430c9-106">You can use [Azure Resource Explorer](https://resources.azure.com) to find the instance ID of a virtual machine in a scale set.</span></span> <span data-ttu-id="430c9-107">Pomocí Průzkumníka prostředků také ověřte stav úlohy, které dokončíte.</span><span class="sxs-lookup"><span data-stu-id="430c9-107">You also use Resource Explorer to verify the status of the tasks that you finish.</span></span>

<span data-ttu-id="430c9-108">Projděte si článek [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview), kde najdete informace o instalaci nejnovější verze prostředí Azure PowerShell, výběru předplatného a přihlášení k účtu.</span><span class="sxs-lookup"><span data-stu-id="430c9-108">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to your account.</span></span>

## <a name="display-information-about-a-scale-set"></a><span data-ttu-id="430c9-109">Zobrazit informace o sadě škálování</span><span class="sxs-lookup"><span data-stu-id="430c9-109">Display information about a scale set</span></span>
<span data-ttu-id="430c9-110">Můžete získat obecné informace o sadě škálování, která je také označována jako zobrazení instance.</span><span class="sxs-lookup"><span data-stu-id="430c9-110">You can get general information about a scale set, which is also referred to as the instance view.</span></span> <span data-ttu-id="430c9-111">Nebo můžete získat podrobnější informace, například informace o prostředcích v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="430c9-111">Or, you can get more specific information, such as information about the resources in the scale set.</span></span>

<span data-ttu-id="430c9-112">Nahraďte hodnoty v uvozovkách název nebo vaší skupině prostředků a škálování nastavení a potom spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="430c9-112">Replace the quoted values with the name or your resource group and scale set and then run the command:</span></span>

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

<span data-ttu-id="430c9-113">Vrátí přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="430c9-113">It returns something like this:</span></span>

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded

<span data-ttu-id="430c9-114">Nahraďte název vaší skupiny a škálování sady prostředků hodnoty v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="430c9-114">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="430c9-115">Nahraďte  *#*  s identifikátorem instance virtuálního počítače, který chcete získat informace a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="430c9-115">Replace *#* with the instance identifier of the virtual machine that you want to get information about and then run it:</span></span>

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="430c9-116">Vrátí něco podobného jako v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="430c9-116">It returns something like this example:</span></span>

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded

## <a name="start-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="430c9-117">Spuštění virtuálního počítače ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="430c9-117">Start a virtual machine in a scale set</span></span>
<span data-ttu-id="430c9-118">Nahraďte název vaší skupiny a škálování sady prostředků hodnoty v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="430c9-118">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="430c9-119">Nahraďte  *#*  s identifikátorem virtuálního počítače, který chcete spustit a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="430c9-119">Replace *#* with the identifier of the virtual machine that you want to start and then run it:</span></span>

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="430c9-120">V Průzkumníku prostředků, vidíme, že stav instance je **systémem**:</span><span class="sxs-lookup"><span data-stu-id="430c9-120">In Resource Explorer, we can see that the status of the instance is **running**:</span></span>

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

<span data-ttu-id="430c9-121">Všechny virtuální počítače můžete spustit v sad není pomocí parametru - InstanceId škálování.</span><span class="sxs-lookup"><span data-stu-id="430c9-121">You can start all the virtual machines in the scale set by not using the -InstanceId parameter.</span></span>

## <a name="stop-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="430c9-122">Zastavit virtuální počítač ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="430c9-122">Stop a virtual machine in a scale set</span></span>
<span data-ttu-id="430c9-123">Nahraďte název vaší skupiny a škálování sady prostředků hodnoty v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="430c9-123">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="430c9-124">Nahraďte  *#*  s identifikátorem virtuálního počítače, který chcete zastavit a pak ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="430c9-124">Replace *#* with the identifier of the virtual machine that you want to stop and then run it:</span></span>

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="430c9-125">V Průzkumníku prostředků, vidíme, že stav instance je **navrácena**:</span><span class="sxs-lookup"><span data-stu-id="430c9-125">In Resource Explorer, we can see that the status of the instance is **deallocated**:</span></span>

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]

<span data-ttu-id="430c9-126">Pokud chcete zastavit virtuální počítač a není zrušit přidělení, použijte parametr - StayProvisioned.</span><span class="sxs-lookup"><span data-stu-id="430c9-126">To stop a virtual machine and not deallocate it, use the -StayProvisioned parameter.</span></span> <span data-ttu-id="430c9-127">Není pomocí parametru - InstanceId můžete zastavit všechny virtuální počítače v sadě.</span><span class="sxs-lookup"><span data-stu-id="430c9-127">You can stop all the virtual machines in the set by not using the -InstanceId parameter.</span></span>

## <a name="restart-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="430c9-128">Restartování virtuálního počítače ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="430c9-128">Restart a virtual machine in a scale set</span></span>
<span data-ttu-id="430c9-129">Nahraďte název vaší skupiny prostředků a byly sadou škálování hodnoty v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="430c9-129">Replace the quoted values with the name of your resource group and the scale set.</span></span> <span data-ttu-id="430c9-130">Nahraďte  *#*  identifikátorem virtuální počítač, který chcete restartovat a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="430c9-130">Replace *#* with the identifier of the virtual machine that you want to restart and then run it:</span></span>

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="430c9-131">Není pomocí parametru - InstanceId se dá restartovat všechny virtuální počítače v sadě.</span><span class="sxs-lookup"><span data-stu-id="430c9-131">You can restart all the virtual machines in the set by not using the -InstanceId parameter.</span></span>

## <a name="remove-a-virtual-machine-from-a-scale-set"></a><span data-ttu-id="430c9-132">Odebrání ze sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="430c9-132">Remove a virtual machine from a scale set</span></span>
<span data-ttu-id="430c9-133">Nahraďte název vaší skupiny prostředků a byly sadou škálování hodnoty v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="430c9-133">Replace the quoted values with the name of your resource group and the scale set.</span></span> <span data-ttu-id="430c9-134">Nahraďte  *#*  identifikátorem virtuální počítač, který chcete odebrat a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="430c9-134">Replace *#* with the identifier of the virtual machine that you want to remove and then run it:</span></span>  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="430c9-135">Škálovací sadu virtuálních počítačů najednou můžete odebrat není pomocí parametru - ID instance.</span><span class="sxs-lookup"><span data-stu-id="430c9-135">You can remove the virtual machine scale set all at once by not using the -InstanceId parameter.</span></span>

## <a name="change-the-capacity-of-a-scale-set"></a><span data-ttu-id="430c9-136">Změna kapacity škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="430c9-136">Change the capacity of a scale set</span></span>
<span data-ttu-id="430c9-137">Můžete přidat nebo odebrat virtuální počítače tak, že změníte kapacita sady.</span><span class="sxs-lookup"><span data-stu-id="430c9-137">You can add or remove virtual machines by changing the capacity of the set.</span></span> <span data-ttu-id="430c9-138">Získáte škálovací sadu, který chcete změnit, nastavte kapacitu na co se má být a aktualizujte měřítka nastavit nové kapacity.</span><span class="sxs-lookup"><span data-stu-id="430c9-138">Get the scale set that you want to change, set the capacity to what you want it to be, and then update the scale set with the new capacity.</span></span> <span data-ttu-id="430c9-139">V těchto příkazech nahraďte hodnoty v uvozovkách název vaší skupiny prostředků a byly sadou škálování.</span><span class="sxs-lookup"><span data-stu-id="430c9-139">In these commands, replace the quoted values with the name of your resource group and the scale set.</span></span>

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

<span data-ttu-id="430c9-140">Pokud odebíráte ze sady škálování virtuálních počítačů, virtuálních počítačů s nejvyšší ID nejdřív odstranit.</span><span class="sxs-lookup"><span data-stu-id="430c9-140">If you are removing virtual machines from the scale set, the virtual machines with the highest ids are removed first.</span></span>

