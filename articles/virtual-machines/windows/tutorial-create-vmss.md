---
title: "aaaCreate virtuální počítač škálování sady pro Windows v Azure | Microsoft Docs"
description: "Vytvoření a nasazení vysoce dostupné aplikace na virtuálních počítačích Windows pomocí sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a3914571005c28a492c069d880992630851afae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Vytvořit sadu škálování virtuálního počítače a nasazení vysoce dostupné aplikace v systému Windows
Škálovací sadu virtuálních počítačů vám umožní toodeploy a spravovat sadu identické, automatické škálování virtuálních počítačů. Můžete škálovat hello počet virtuálních počítačů v sadě škálování hello ručně nebo definovat tooautoscale pravidla na základě využití procesoru, paměti vyžádání nebo síťový provoz. V tomto kurzu nasadíte škálování virtuálních počítačů, nastavte v Azure. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Použít hello rozšíření vlastních skriptů toodefine tooscale webu služby IIS
> * Vytvořit nástroj pro vyrovnávání zatížení pro škálovací sadu
> * Vytvoření sady škálování virtuálního počítače
> * Zvýšení nebo snížení hello počet instancí v sadě škálování
> * Vytvoření pravidel škálování

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="scale-set-overview"></a>Přehled sady škálování
Sady škálování použijte podobné koncepty, jak jste se naučili o v předchozí kurzu hello příliš[vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md). Virtuální počítače ve škálovací sadě jsou rozmístěny v doménách selhání a aktualizace stejně jako virtuální počítače v nastavení dostupnosti.

Podle potřeby ve škálovací sadě virtuálních počítačů vytvořeny. Můžete definovat pravidla toocontrol škálování jak a kdy se přidají nebo odeberou z hello škálovací sadu virtuálních počítačů. Tato pravidla můžete spustit na základě metriky například zatížení procesoru, využití paměti nebo síťový provoz.

Škálování nastaví podporu až too1 000 virtuálních počítačů, pokud používáte image platformy Azure. Pro úlohy s významné instalace nebo požadavky přizpůsobení virtuálního počítače, můžete příliš[vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md). Můžete vytvořit až too100 virtuálních počítačů v škálování nastavit při použití vlastní image.


## <a name="create-an-app-tooscale"></a>Vytvoření tooscale aplikaci
Před vytvořením sady škálování, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v hello *EastUS* umístění:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

V dřívějších kurzu, jste se dozvěděli, jak příliš[automatizovat konfiguraci](tutorial-automate-vm-deployment.md) pomocí hello rozšíření vlastních skriptů. Vytvoření konfigurace sady škálování pak použijte tooinstall rozšíření vlastních skriptů a konfiguraci služby IIS:

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define hello script for your Custom Script Extension toorun
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension tooinstall IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení škálování
K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku. Stav sondu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a pouze distribuuje provoz tooan provozní virtuálních počítačů. Další informace najdete v tématu hello další kurz na [jak tooload vyvážit virtuálními počítači s Windows](tutorial-load-balancer.md).

Vytvořte Vyrovnávání zatížení, která má veřejnou IP adresu a distribuuje webové přenosy na portu 80:

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create hello load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule toodistribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update hello load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>Vytvoření sady škálování
Nyní vytvoří škálování virtuálního počítače s [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm). Hello následující příklad vytvoří škálování nastavení s názvem *myScaleSet*:

```powershell
# Reference a virtual machine image from hello gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with hello virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create hello virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach hello virtual network toohello config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create hello scale set with hello config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

Přebírá toocreate pár minut a konfigurace všech hello škálování sadu prostředků a virtuální počítače.


## <a name="test-your-app"></a>Testování aplikace
toosee váš web služby IIS v akci, získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Hello následující příklad získá hello IP adresu pro *myPublicIP* vytvořen jako součást sady škálování hello:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Zadejte ve webovém prohlížeči tooa hello veřejnou IP adresu. Zobrazí se aplikace Hello, včetně hello název hostitele virtuálních počítačů této hello načíst vyrovnávání distribuované provoz hello:

![Spuštěné web služby IIS](./media/tutorial-create-vmss/running-iis-site.png)

sad v akci škálování hello toosee, můžete můžete vynutit obnovení webové prohlížeče toosee hello zatížení vyrovnávání provoz distribuovat do všech virtuálních počítačů hello používající vaši aplikaci.


## <a name="management-tasks"></a>Úlohy správy
V průběhu cyklu hello škálovací sady hello, může být nutné toorun jedné nebo více úloh správy. Kromě toho můžete toocreate skripty, které automatizují různé úlohy životního cyklu. Azure PowerShell poskytuje rychlý způsob toodo tyto úlohy. Tady jsou několik běžných úloh.

### <a name="view-vms-in-a-scale-set"></a>Zobrazení virtuální počítače ve škálovací sadě
nastavit seznam virtuálních počítačů spuštěných ve vašem škálování tooview, použijte [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) následujícím způsobem:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through hello instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>Zvýšení nebo snížení instance virtuálních počítačů
toosee hello počet instancí aktuálně v škálování nastavíte, použijte [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) a dotazovat se na *sku.capacity*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

Potom můžete ručně zvýšit nebo snížit hello počet virtuálních počítačů v hello škálování s [aktualizace AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). Hello následující příklad nastaví hello počet virtuálních počítačů ve vaší příliš sad škálování*5*:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update hello capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

Pokud trvá několik minut tooupdate hello zadaný počet instancí ve vaší sadě škálování.


### <a name="configure-autoscale-rules"></a>Konfigurace pravidel automatického škálování
Místo ručně škálování hello počet instancí ve vaší škálovací sady, můžete definovat pravidla automatického škálování. Tato pravidla monitorování hello instancí ve vašem škálování nastavit a reagují odpovídajícím způsobem na základě metriky a prahové hodnoty, které definujete. Následující ukázka Hello škáluje hello snížit počet instancí jedním při hello průměrné zatížení procesoru je větší než 60 % během období 5 minut. Pokud hello průměr procesoru zatížení pak klesne pod 30 % během období 5 minut, jsou instance hello škálovat v jednou instancí:

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule tooincrease hello number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule toodecrease hello number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply hello autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste vytvořili škálovací sadu virtuálních počítačů. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Použít hello rozšíření vlastních skriptů toodefine tooscale webu služby IIS
> * Vytvořit nástroj pro vyrovnávání zatížení pro škálovací sadu
> * Vytvoření sady škálování virtuálního počítače
> * Zvýšení nebo snížení hello počet instancí v sadě škálování
> * Vytvoření pravidel škálování

Posunutí toohello další kurz toolearn informace o konceptech pro virtuální počítače Vyrovnávání zatížení.

> [!div class="nextstepaction"]
> [Vyrovnávat zatížení virtuálních počítačů](tutorial-load-balancer.md)
