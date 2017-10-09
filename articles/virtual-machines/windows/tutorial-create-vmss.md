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
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a><span data-ttu-id="2550b-103">Vytvořit sadu škálování virtuálního počítače a nasazení vysoce dostupné aplikace v systému Windows</span><span class="sxs-lookup"><span data-stu-id="2550b-103">Create a Virtual Machine Scale Set and deploy a highly available app on Windows</span></span>
<span data-ttu-id="2550b-104">Škálovací sadu virtuálních počítačů vám umožní toodeploy a spravovat sadu identické, automatické škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2550b-104">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="2550b-105">Můžete škálovat hello počet virtuálních počítačů v sadě škálování hello ručně nebo definovat tooautoscale pravidla na základě využití procesoru, paměti vyžádání nebo síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="2550b-105">You can scale hello number of VMs in hello scale set manually, or define rules tooautoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="2550b-106">V tomto kurzu nasadíte škálování virtuálních počítačů, nastavte v Azure.</span><span class="sxs-lookup"><span data-stu-id="2550b-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="2550b-107">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="2550b-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2550b-108">Použít hello rozšíření vlastních skriptů toodefine tooscale webu služby IIS</span><span class="sxs-lookup"><span data-stu-id="2550b-108">Use hello Custom Script Extension toodefine an IIS site tooscale</span></span>
> * <span data-ttu-id="2550b-109">Vytvořit nástroj pro vyrovnávání zatížení pro škálovací sadu</span><span class="sxs-lookup"><span data-stu-id="2550b-109">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="2550b-110">Vytvoření sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2550b-110">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="2550b-111">Zvýšení nebo snížení hello počet instancí v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="2550b-111">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="2550b-112">Vytvoření pravidel škálování</span><span class="sxs-lookup"><span data-stu-id="2550b-112">Create autoscale rules</span></span>

<span data-ttu-id="2550b-113">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2550b-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="2550b-114">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="2550b-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="2550b-115">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="2550b-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="scale-set-overview"></a><span data-ttu-id="2550b-116">Přehled sady škálování</span><span class="sxs-lookup"><span data-stu-id="2550b-116">Scale Set overview</span></span>
<span data-ttu-id="2550b-117">Sady škálování použijte podobné koncepty, jak jste se naučili o v předchozí kurzu hello příliš[vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="2550b-117">Scale sets use similar concepts as you learned about in hello previous tutorial too[Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="2550b-118">Virtuální počítače ve škálovací sadě jsou rozmístěny v doménách selhání a aktualizace stejně jako virtuální počítače v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2550b-118">VMs in a scale set are distributed across fault and update domains just like VMs in an availability set.</span></span>

<span data-ttu-id="2550b-119">Podle potřeby ve škálovací sadě virtuálních počítačů vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="2550b-119">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="2550b-120">Můžete definovat pravidla toocontrol škálování jak a kdy se přidají nebo odeberou z hello škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2550b-120">You define autoscale rules toocontrol how and when VMs are added or removed from hello scale set.</span></span> <span data-ttu-id="2550b-121">Tato pravidla můžete spustit na základě metriky například zatížení procesoru, využití paměti nebo síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="2550b-121">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="2550b-122">Škálování nastaví podporu až too1 000 virtuálních počítačů, pokud používáte image platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="2550b-122">Scale sets support up too1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="2550b-123">Pro úlohy s významné instalace nebo požadavky přizpůsobení virtuálního počítače, můžete příliš[vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="2550b-123">For workloads with significant installation or VM customization requirements, you may wish too[Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="2550b-124">Můžete vytvořit až too100 virtuálních počítačů v škálování nastavit při použití vlastní image.</span><span class="sxs-lookup"><span data-stu-id="2550b-124">You can create up too100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-tooscale"></a><span data-ttu-id="2550b-125">Vytvoření tooscale aplikaci</span><span class="sxs-lookup"><span data-stu-id="2550b-125">Create an app tooscale</span></span>
<span data-ttu-id="2550b-126">Před vytvořením sady škálování, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="2550b-126">Before you can create a scale set, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="2550b-127">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v hello *EastUS* umístění:</span><span class="sxs-lookup"><span data-stu-id="2550b-127">hello following example creates a resource group named *myResourceGroupAutomate* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

<span data-ttu-id="2550b-128">V dřívějších kurzu, jste se dozvěděli, jak příliš[automatizovat konfiguraci](tutorial-automate-vm-deployment.md) pomocí hello rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="2550b-128">In an earlier tutorial, you learned how too[Automate VM configuration](tutorial-automate-vm-deployment.md) using hello Custom Script Extension.</span></span> <span data-ttu-id="2550b-129">Vytvoření konfigurace sady škálování pak použijte tooinstall rozšíření vlastních skriptů a konfiguraci služby IIS:</span><span class="sxs-lookup"><span data-stu-id="2550b-129">Create a scale set configuration then apply a Custom Script Extension tooinstall and configure IIS:</span></span>

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

## <a name="create-scale-load-balancer"></a><span data-ttu-id="2550b-130">Vytvořit nástroj pro vyrovnávání zatížení škálování</span><span class="sxs-lookup"><span data-stu-id="2550b-130">Create scale load balancer</span></span>
<span data-ttu-id="2550b-131">K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku.</span><span class="sxs-lookup"><span data-stu-id="2550b-131">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="2550b-132">Stav sondu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a pouze distribuuje provoz tooan provozní virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2550b-132">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span> <span data-ttu-id="2550b-133">Další informace najdete v tématu hello další kurz na [jak tooload vyvážit virtuálními počítači s Windows](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="2550b-133">For more information, see hello next tutorial on [How tooload balance Windows virtual machines](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="2550b-134">Vytvořte Vyrovnávání zatížení, která má veřejnou IP adresu a distribuuje webové přenosy na portu 80:</span><span class="sxs-lookup"><span data-stu-id="2550b-134">Create a load balancer that has a public IP address and distributes web traffic on port 80:</span></span>

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

## <a name="create-a-scale-set"></a><span data-ttu-id="2550b-135">Vytvoření sady škálování</span><span class="sxs-lookup"><span data-stu-id="2550b-135">Create a scale set</span></span>
<span data-ttu-id="2550b-136">Nyní vytvoří škálování virtuálního počítače s [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="2550b-136">Now create a virtual machine scale set with [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="2550b-137">Hello následující příklad vytvoří škálování nastavení s názvem *myScaleSet*:</span><span class="sxs-lookup"><span data-stu-id="2550b-137">hello following example creates a scale set named *myScaleSet*:</span></span>

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

<span data-ttu-id="2550b-138">Přebírá toocreate pár minut a konfigurace všech hello škálování sadu prostředků a virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="2550b-138">It takes a few minutes toocreate and configure all hello scale set resources and VMs.</span></span>


## <a name="test-your-app"></a><span data-ttu-id="2550b-139">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="2550b-139">Test your app</span></span>
<span data-ttu-id="2550b-140">toosee váš web služby IIS v akci, získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="2550b-140">toosee your IIS website in action, obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="2550b-141">Hello následující příklad získá hello IP adresu pro *myPublicIP* vytvořen jako součást sady škálování hello:</span><span class="sxs-lookup"><span data-stu-id="2550b-141">hello following example obtains hello IP address for *myPublicIP* created as part of hello scale set:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

<span data-ttu-id="2550b-142">Zadejte ve webovém prohlížeči tooa hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2550b-142">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="2550b-143">Zobrazí se aplikace Hello, včetně hello název hostitele virtuálních počítačů této hello načíst vyrovnávání distribuované provoz hello:</span><span class="sxs-lookup"><span data-stu-id="2550b-143">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic to:</span></span>

![Spuštěné web služby IIS](./media/tutorial-create-vmss/running-iis-site.png)

<span data-ttu-id="2550b-145">sad v akci škálování hello toosee, můžete můžete vynutit obnovení webové prohlížeče toosee hello zatížení vyrovnávání provoz distribuovat do všech virtuálních počítačů hello používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2550b-145">toosee hello scale set in action, you can force-refresh your web browser toosee hello load balancer distribute traffic across all hello VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="2550b-146">Úlohy správy</span><span class="sxs-lookup"><span data-stu-id="2550b-146">Management tasks</span></span>
<span data-ttu-id="2550b-147">V průběhu cyklu hello škálovací sady hello, může být nutné toorun jedné nebo více úloh správy.</span><span class="sxs-lookup"><span data-stu-id="2550b-147">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="2550b-148">Kromě toho můžete toocreate skripty, které automatizují různé úlohy životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="2550b-148">Additionally, you may want toocreate scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="2550b-149">Azure PowerShell poskytuje rychlý způsob toodo tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="2550b-149">Azure PowerShell provides a quick way toodo those tasks.</span></span> <span data-ttu-id="2550b-150">Tady jsou několik běžných úloh.</span><span class="sxs-lookup"><span data-stu-id="2550b-150">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="2550b-151">Zobrazení virtuální počítače ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="2550b-151">View VMs in a scale set</span></span>
<span data-ttu-id="2550b-152">nastavit seznam virtuálních počítačů spuštěných ve vašem škálování tooview, použijte [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2550b-152">tooview a list of VMs running in your scale set, use [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) as follows:</span></span>

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


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="2550b-153">Zvýšení nebo snížení instance virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="2550b-153">Increase or decrease VM instances</span></span>
<span data-ttu-id="2550b-154">toosee hello počet instancí aktuálně v škálování nastavíte, použijte [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) a dotazovat se na *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="2550b-154">toosee hello number of instances you currently have in a scale set, use [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) and query on *sku.capacity*:</span></span>

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

<span data-ttu-id="2550b-155">Potom můžete ručně zvýšit nebo snížit hello počet virtuálních počítačů v hello škálování s [aktualizace AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span><span class="sxs-lookup"><span data-stu-id="2550b-155">You can then manually increase or decrease hello number of virtual machines in hello scale set with [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span></span> <span data-ttu-id="2550b-156">Hello následující příklad nastaví hello počet virtuálních počítačů ve vaší příliš sad škálování*5*:</span><span class="sxs-lookup"><span data-stu-id="2550b-156">hello following example sets hello number of VMs in your scale set too*5*:</span></span>

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

<span data-ttu-id="2550b-157">Pokud trvá několik minut tooupdate hello zadaný počet instancí ve vaší sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="2550b-157">If takes a few minutes tooupdate hello specified number of instances in your scale set.</span></span>


### <a name="configure-autoscale-rules"></a><span data-ttu-id="2550b-158">Konfigurace pravidel automatického škálování</span><span class="sxs-lookup"><span data-stu-id="2550b-158">Configure autoscale rules</span></span>
<span data-ttu-id="2550b-159">Místo ručně škálování hello počet instancí ve vaší škálovací sady, můžete definovat pravidla automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="2550b-159">Rather than manually scaling hello number of instances in your scale set, you define autoscale rules.</span></span> <span data-ttu-id="2550b-160">Tato pravidla monitorování hello instancí ve vašem škálování nastavit a reagují odpovídajícím způsobem na základě metriky a prahové hodnoty, které definujete.</span><span class="sxs-lookup"><span data-stu-id="2550b-160">These rules monitor hello instances in your scale set and respond accordingly based on metrics and thresholds you define.</span></span> <span data-ttu-id="2550b-161">Následující ukázka Hello škáluje hello snížit počet instancí jedním při hello průměrné zatížení procesoru je větší než 60 % během období 5 minut.</span><span class="sxs-lookup"><span data-stu-id="2550b-161">hello following example scales out hello number of instances by one when hello average CPU load is greater than 60% over a 5 minute period.</span></span> <span data-ttu-id="2550b-162">Pokud hello průměr procesoru zatížení pak klesne pod 30 % během období 5 minut, jsou instance hello škálovat v jednou instancí:</span><span class="sxs-lookup"><span data-stu-id="2550b-162">If hello average CPU load then drops below 30% over a 5 minute period, hello instances are scaled in by one instance:</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="2550b-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2550b-163">Next steps</span></span>
<span data-ttu-id="2550b-164">V tomto kurzu jste vytvořili škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2550b-164">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="2550b-165">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="2550b-165">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2550b-166">Použít hello rozšíření vlastních skriptů toodefine tooscale webu služby IIS</span><span class="sxs-lookup"><span data-stu-id="2550b-166">Use hello Custom Script Extension toodefine an IIS site tooscale</span></span>
> * <span data-ttu-id="2550b-167">Vytvořit nástroj pro vyrovnávání zatížení pro škálovací sadu</span><span class="sxs-lookup"><span data-stu-id="2550b-167">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="2550b-168">Vytvoření sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2550b-168">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="2550b-169">Zvýšení nebo snížení hello počet instancí v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="2550b-169">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="2550b-170">Vytvoření pravidel škálování</span><span class="sxs-lookup"><span data-stu-id="2550b-170">Create autoscale rules</span></span>

<span data-ttu-id="2550b-171">Posunutí toohello další kurz toolearn informace o konceptech pro virtuální počítače Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2550b-171">Advance toohello next tutorial toolearn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2550b-172">Vyrovnávat zatížení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="2550b-172">Load balance virtual machines</span></span>](tutorial-load-balancer.md)
