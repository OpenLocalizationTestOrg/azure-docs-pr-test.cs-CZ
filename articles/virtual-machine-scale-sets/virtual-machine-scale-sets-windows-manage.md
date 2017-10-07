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
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Spravovat virtuální počítače ve škálovací sadě virtuálních počítačů
Použití hello úloh v tomto článku toomanage virtuální počítače sady škálování virtuálního počítače.

Většina hello úlohy, které zahrnují spravuje virtuální počítač ve škálovací sadě vyžadovat, že znáte ID instance hello hello počítače, které chcete toomanage. Můžete použít [Průzkumníka prostředků Azure](https://resources.azure.com) toofind hello ID instance virtuálního počítače ve škálovací sadě. Je také použít Průzkumníka prostředků tooverify hello stav hello úlohy, které dokončíte.

V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooyour účtu.

## <a name="display-information-about-a-scale-set"></a>Zobrazit informace o sadě škálování
Obecné informace o sadě škálování, který je taky zobrazení instance hello odkazované tooas můžete získat. Nebo můžete získat podrobnější informace, například informace o prostředcích hello v sadě škálování hello.

Nahraďte hodnoty s názvem hello nebo vaší skupině prostředků a škálování nastavení a potom spusťte příkaz hello v uvozovkách hello:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Vrátí přibližně takto:

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

Nahraďte hello hodnoty s hello název skupiny a škálování sady prostředků v uvozovkách. Nahraďte  *#*  s identifikátorem instance hello hello virtuálního počítače o chcete tooget informace a potom ho spusťte:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Vrátí něco podobného jako v tomto příkladu:

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

## <a name="start-a-virtual-machine-in-a-scale-set"></a>Spuštění virtuálního počítače ve škálovací sadě
Nahraďte hello hodnoty s hello název skupiny a škálování sady prostředků v uvozovkách. Nahraďte  *#*  s identifikátorem hello hello virtuálního počítače má toostart a potom ho spusťte:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

V Průzkumníku prostředků, vidíme, že stav hello hello instance je **systémem**:

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

Všechny virtuální počítače hello můžete spustit v sad není pomocí parametru - InstanceId hello hello škálování.

## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Zastavit virtuální počítač ve škálovací sadě
Nahraďte hello hodnoty s hello název skupiny a škálování sady prostředků v uvozovkách. Nahraďte  *#*  s identifikátorem hello hello virtuálního počítače má toostop a potom ho spusťte:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

V Průzkumníku prostředků, vidíme, že stav hello hello instance je **navrácena**:

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

toostop virtuálního počítače není navrácení ji, použijte parametr - StayProvisioned hello. Můžete zastavit všechny virtuální počítače hello v hello nastavit není pomocí parametru - InstanceId hello.

## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Restartování virtuálního počítače ve škálovací sadě
Nahraďte hello hodnoty s hello název vaší sady prostředků skupiny a hello škálování v uvozovkách. Nahraďte  *#*  s identifikátorem hello hello virtuálního počítače má toorestart a potom ho spusťte:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Všechny virtuální počítače hello můžete restartovat v hello nastavit není pomocí parametru - InstanceId hello.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Odebrání ze sady škálování virtuálního počítače
Nahraďte hello hodnoty s hello název vaší sady prostředků skupiny a hello škálování v uvozovkách. Nahraďte  *#*  s identifikátorem hello hello virtuálního počítače má tooremove a potom ho spusťte:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Hello škálovací sadu virtuálních počítačů najednou můžete odebrat není pomocí parametru - InstanceId hello.

## <a name="change-hello-capacity-of-a-scale-set"></a>Změna hello kapacity škálovací sadě
Můžete přidat nebo odebrat virtuální počítače tak, že změníte hello kapacita sady hello. Získáte škálovací sadu hello, který chcete toochange, sada hello kapacity toowhat chcete ho toobe a potom aktualizovat sadu škálování hello s hello nové kapacity. V těchto příkazech nahraďte hello hodnoty s hello název vaší sady prostředků skupiny a hello škálování v uvozovkách.

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

Pokud odebíráte z hello škálovací sadu virtuálních počítačů, se nejprve odebrat hello virtuální počítače s nejvyšší ID hello.

