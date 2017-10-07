---
title: "aaaManage virtuálních počítačů v sadě škálování virtuálního počítače | Microsoft Docs"
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
ms.openlocfilehash: 7d848729c0fc708bd596b61feb528cf4bf4bafd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="e6d72-103">Spravovat virtuální počítače ve škálovací sadě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e6d72-103">Manage virtual machines in a virtual machine scale set</span></span>
<span data-ttu-id="e6d72-104">Použití hello úloh v tomto článku toomanage virtuální počítače sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6d72-104">Use hello tasks in this article toomanage virtual machines in your virtual machine scale set.</span></span>

<span data-ttu-id="e6d72-105">Většina hello úlohy, které zahrnují spravuje virtuální počítač ve škálovací sadě vyžadovat, že znáte ID instance hello hello počítače, které chcete toomanage.</span><span class="sxs-lookup"><span data-stu-id="e6d72-105">Most of hello tasks that involve managing a virtual machine in a scale set require that you know hello instance ID of hello machine that you want toomanage.</span></span> <span data-ttu-id="e6d72-106">Můžete použít [Průzkumníka prostředků Azure](https://resources.azure.com) toofind hello ID instance virtuálního počítače ve škálovací sadě.</span><span class="sxs-lookup"><span data-stu-id="e6d72-106">You can use [Azure Resource Explorer](https://resources.azure.com) toofind hello instance ID of a virtual machine in a scale set.</span></span> <span data-ttu-id="e6d72-107">Je také použít Průzkumníka prostředků tooverify hello stav hello úlohy, které dokončíte.</span><span class="sxs-lookup"><span data-stu-id="e6d72-107">You also use Resource Explorer tooverify hello status of hello tasks that you finish.</span></span>

<span data-ttu-id="e6d72-108">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="e6d72-108">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooyour account.</span></span>

## <a name="display-information-about-a-scale-set"></a><span data-ttu-id="e6d72-109">Zobrazit informace o sadě škálování</span><span class="sxs-lookup"><span data-stu-id="e6d72-109">Display information about a scale set</span></span>
<span data-ttu-id="e6d72-110">Obecné informace o sadě škálování, který je taky zobrazení instance hello odkazované tooas můžete získat.</span><span class="sxs-lookup"><span data-stu-id="e6d72-110">You can get general information about a scale set, which is also referred tooas hello instance view.</span></span> <span data-ttu-id="e6d72-111">Nebo můžete získat podrobnější informace, například informace o prostředcích hello v sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="e6d72-111">Or, you can get more specific information, such as information about hello resources in hello scale set.</span></span>

<span data-ttu-id="e6d72-112">Nahraďte hodnoty s názvem hello nebo vaší skupině prostředků a škálování nastavení a potom spusťte příkaz hello v uvozovkách hello:</span><span class="sxs-lookup"><span data-stu-id="e6d72-112">Replace hello quoted values with hello name or your resource group and scale set and then run hello command:</span></span>

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

<span data-ttu-id="e6d72-113">Vrátí přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="e6d72-113">It returns something like this:</span></span>

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

<span data-ttu-id="e6d72-114">Nahraďte hello hodnoty s hello název skupiny a škálování sady prostředků v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="e6d72-114">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="e6d72-115">Nahraďte  *#*  s identifikátorem instance hello hello virtuálního počítače o chcete tooget informace a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="e6d72-115">Replace *#* with hello instance identifier of hello virtual machine that you want tooget information about and then run it:</span></span>

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="e6d72-116">Vrátí něco podobného jako v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="e6d72-116">It returns something like this example:</span></span>

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

## <a name="start-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="e6d72-117">Spuštění virtuálního počítače ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="e6d72-117">Start a virtual machine in a scale set</span></span>
<span data-ttu-id="e6d72-118">Nahraďte hello hodnoty s hello název skupiny a škálování sady prostředků v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="e6d72-118">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="e6d72-119">Nahraďte  *#*  s identifikátorem hello hello virtuálního počítače má toostart a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="e6d72-119">Replace *#* with hello identifier of hello virtual machine that you want toostart and then run it:</span></span>

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="e6d72-120">V Průzkumníku prostředků, vidíme, že stav hello hello instance je **systémem**:</span><span class="sxs-lookup"><span data-stu-id="e6d72-120">In Resource Explorer, we can see that hello status of hello instance is **running**:</span></span>

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

<span data-ttu-id="e6d72-121">Všechny virtuální počítače hello můžete spustit v sad není pomocí parametru - InstanceId hello hello škálování.</span><span class="sxs-lookup"><span data-stu-id="e6d72-121">You can start all hello virtual machines in hello scale set by not using hello -InstanceId parameter.</span></span>

## <a name="stop-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="e6d72-122">Zastavit virtuální počítač ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="e6d72-122">Stop a virtual machine in a scale set</span></span>
<span data-ttu-id="e6d72-123">Nahraďte hello hodnoty s hello název skupiny a škálování sady prostředků v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="e6d72-123">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="e6d72-124">Nahraďte  *#*  s identifikátorem hello hello virtuálního počítače má toostop a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="e6d72-124">Replace *#* with hello identifier of hello virtual machine that you want toostop and then run it:</span></span>

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="e6d72-125">V Průzkumníku prostředků, vidíme, že stav hello hello instance je **navrácena**:</span><span class="sxs-lookup"><span data-stu-id="e6d72-125">In Resource Explorer, we can see that hello status of hello instance is **deallocated**:</span></span>

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

<span data-ttu-id="e6d72-126">toostop virtuálního počítače není navrácení ji, použijte parametr - StayProvisioned hello.</span><span class="sxs-lookup"><span data-stu-id="e6d72-126">toostop a virtual machine and not deallocate it, use hello -StayProvisioned parameter.</span></span> <span data-ttu-id="e6d72-127">Můžete zastavit všechny virtuální počítače hello v hello nastavit není pomocí parametru - InstanceId hello.</span><span class="sxs-lookup"><span data-stu-id="e6d72-127">You can stop all hello virtual machines in hello set by not using hello -InstanceId parameter.</span></span>

## <a name="restart-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="e6d72-128">Restartování virtuálního počítače ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="e6d72-128">Restart a virtual machine in a scale set</span></span>
<span data-ttu-id="e6d72-129">Nahraďte hello hodnoty s hello název vaší sady prostředků skupiny a hello škálování v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="e6d72-129">Replace hello quoted values with hello name of your resource group and hello scale set.</span></span> <span data-ttu-id="e6d72-130">Nahraďte  *#*  s identifikátorem hello hello virtuálního počítače má toorestart a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="e6d72-130">Replace *#* with hello identifier of hello virtual machine that you want toorestart and then run it:</span></span>

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="e6d72-131">Všechny virtuální počítače hello můžete restartovat v hello nastavit není pomocí parametru - InstanceId hello.</span><span class="sxs-lookup"><span data-stu-id="e6d72-131">You can restart all hello virtual machines in hello set by not using hello -InstanceId parameter.</span></span>

## <a name="remove-a-virtual-machine-from-a-scale-set"></a><span data-ttu-id="e6d72-132">Odebrání ze sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e6d72-132">Remove a virtual machine from a scale set</span></span>
<span data-ttu-id="e6d72-133">Nahraďte hello hodnoty s hello název vaší sady prostředků skupiny a hello škálování v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="e6d72-133">Replace hello quoted values with hello name of your resource group and hello scale set.</span></span> <span data-ttu-id="e6d72-134">Nahraďte  *#*  s identifikátorem hello hello virtuálního počítače má tooremove a potom ho spusťte:</span><span class="sxs-lookup"><span data-stu-id="e6d72-134">Replace *#* with hello identifier of hello virtual machine that you want tooremove and then run it:</span></span>  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="e6d72-135">Hello škálovací sadu virtuálních počítačů najednou můžete odebrat není pomocí parametru - InstanceId hello.</span><span class="sxs-lookup"><span data-stu-id="e6d72-135">You can remove hello virtual machine scale set all at once by not using hello -InstanceId parameter.</span></span>

## <a name="change-hello-capacity-of-a-scale-set"></a><span data-ttu-id="e6d72-136">Změna hello kapacity škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="e6d72-136">Change hello capacity of a scale set</span></span>
<span data-ttu-id="e6d72-137">Můžete přidat nebo odebrat virtuální počítače tak, že změníte hello kapacita sady hello.</span><span class="sxs-lookup"><span data-stu-id="e6d72-137">You can add or remove virtual machines by changing hello capacity of hello set.</span></span> <span data-ttu-id="e6d72-138">Získáte škálovací sadu hello, který chcete toochange, sada hello kapacity toowhat chcete ho toobe a potom aktualizovat sadu škálování hello s hello nové kapacity.</span><span class="sxs-lookup"><span data-stu-id="e6d72-138">Get hello scale set that you want toochange, set hello capacity toowhat you want it toobe, and then update hello scale set with hello new capacity.</span></span> <span data-ttu-id="e6d72-139">V těchto příkazech nahraďte hello hodnoty s hello název vaší sady prostředků skupiny a hello škálování v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="e6d72-139">In these commands, replace hello quoted values with hello name of your resource group and hello scale set.</span></span>

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

<span data-ttu-id="e6d72-140">Pokud odebíráte z hello škálovací sadu virtuálních počítačů, se nejprve odebrat hello virtuální počítače s nejvyšší ID hello.</span><span class="sxs-lookup"><span data-stu-id="e6d72-140">If you are removing virtual machines from hello scale set, hello virtual machines with hello highest ids are removed first.</span></span>

