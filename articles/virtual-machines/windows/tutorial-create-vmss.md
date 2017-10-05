---
title: "Vytvoření sady škálování virtuálního počítače pro Windows v Azure | Microsoft Docs"
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
ms.openlocfilehash: 6600d3b401495f6c291249935b16bcc36c9b8f4b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a><span data-ttu-id="df7c9-103">Vytvořit sadu škálování virtuálního počítače a nasazení vysoce dostupné aplikace v systému Windows</span><span class="sxs-lookup"><span data-stu-id="df7c9-103">Create a Virtual Machine Scale Set and deploy a highly available app on Windows</span></span>
<span data-ttu-id="df7c9-104">Škálovací sadu virtuálních počítačů můžete nasadit a spravovat sadu identické, automatické škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="df7c9-104">A virtual machine scale set allows you to deploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="df7c9-105">Můžete škálovat počet virtuálních počítačů v sadě škálování ručně, nebo definovat pravidla pro automatické škálování podle využití procesoru, paměti vyžádání nebo síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="df7c9-105">You can scale the number of VMs in the scale set manually, or define rules to autoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="df7c9-106">V tomto kurzu nasadíte škálování virtuálních počítačů, nastavte v Azure.</span><span class="sxs-lookup"><span data-stu-id="df7c9-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="df7c9-107">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="df7c9-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="df7c9-108">Použijte k definování stránku IIS škálování rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="df7c9-108">Use the Custom Script Extension to define an IIS site to scale</span></span>
> * <span data-ttu-id="df7c9-109">Vytvořit nástroj pro vyrovnávání zatížení pro škálovací sadu</span><span class="sxs-lookup"><span data-stu-id="df7c9-109">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="df7c9-110">Vytvoření sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="df7c9-110">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="df7c9-111">Zvýšení nebo snížení počtu instancí v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-111">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="df7c9-112">Vytvoření pravidel škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-112">Create autoscale rules</span></span>

<span data-ttu-id="df7c9-113">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="df7c9-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="df7c9-114">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="df7c9-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="df7c9-115">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="df7c9-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="scale-set-overview"></a><span data-ttu-id="df7c9-116">Přehled sady škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-116">Scale Set overview</span></span>
<span data-ttu-id="df7c9-117">Sady škálování použijte podobné koncepty, jak jste se naučili o v předchozí kurzu, který [vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="df7c9-117">Scale sets use similar concepts as you learned about in the previous tutorial to [Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="df7c9-118">Virtuální počítače ve škálovací sadě jsou rozmístěny v doménách selhání a aktualizace stejně jako virtuální počítače v nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="df7c9-118">VMs in a scale set are distributed across fault and update domains just like VMs in an availability set.</span></span>

<span data-ttu-id="df7c9-119">Podle potřeby ve škálovací sadě virtuálních počítačů vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="df7c9-119">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="df7c9-120">Můžete definovat pravidla škálování řídit, jak a kdy se přidají nebo odeberou ze sady škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="df7c9-120">You define autoscale rules to control how and when VMs are added or removed from the scale set.</span></span> <span data-ttu-id="df7c9-121">Tato pravidla můžete spustit na základě metriky například zatížení procesoru, využití paměti nebo síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="df7c9-121">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="df7c9-122">Pokud používáte image platformy Azure sadách škálování podporu až 1 000 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="df7c9-122">Scale sets support up to 1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="df7c9-123">Pro úlohy s významné instalace nebo požadavky přizpůsobení virtuálního počítače, můžete chtít [vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="df7c9-123">For workloads with significant installation or VM customization requirements, you may wish to [Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="df7c9-124">Můžete vytvořit až 100 virtuálními počítači v škálování nastavit při použití vlastní image.</span><span class="sxs-lookup"><span data-stu-id="df7c9-124">You can create up to 100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-to-scale"></a><span data-ttu-id="df7c9-125">Vytvoření aplikace pro škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-125">Create an app to scale</span></span>
<span data-ttu-id="df7c9-126">Před vytvořením sady škálování, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="df7c9-126">Before you can create a scale set, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="df7c9-127">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v *EastUS* umístění:</span><span class="sxs-lookup"><span data-stu-id="df7c9-127">The following example creates a resource group named *myResourceGroupAutomate* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

<span data-ttu-id="df7c9-128">V dřívějších kurzu, jste zjistili, jak [automatizovat konfiguraci](tutorial-automate-vm-deployment.md) pomocí rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="df7c9-128">In an earlier tutorial, you learned how to [Automate VM configuration](tutorial-automate-vm-deployment.md) using the Custom Script Extension.</span></span> <span data-ttu-id="df7c9-129">Vytvoření konfigurace sady škálování a poté použít rozšíření vlastních skriptů k instalaci a konfiguraci služby IIS:</span><span class="sxs-lookup"><span data-stu-id="df7c9-129">Create a scale set configuration then apply a Custom Script Extension to install and configure IIS:</span></span>

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a><span data-ttu-id="df7c9-130">Vytvořit nástroj pro vyrovnávání zatížení škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-130">Create scale load balancer</span></span>
<span data-ttu-id="df7c9-131">K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku.</span><span class="sxs-lookup"><span data-stu-id="df7c9-131">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="df7c9-132">Sondu stavu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom přenosy na provozní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="df7c9-132">A load balancer health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span> <span data-ttu-id="df7c9-133">Další informace najdete v dalším kurzu na [jak načíst vyvážit virtuální počítače s Windows](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="df7c9-133">For more information, see the next tutorial on [How to load balance Windows virtual machines](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="df7c9-134">Vytvořte Vyrovnávání zatížení, která má veřejnou IP adresu a distribuuje webové přenosy na portu 80:</span><span class="sxs-lookup"><span data-stu-id="df7c9-134">Create a load balancer that has a public IP address and distributes web traffic on port 80:</span></span>

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

# Create the load balancer
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

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a><span data-ttu-id="df7c9-135">Vytvoření sady škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-135">Create a scale set</span></span>
<span data-ttu-id="df7c9-136">Nyní vytvoří škálování virtuálního počítače s [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="df7c9-136">Now create a virtual machine scale set with [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="df7c9-137">Následující příklad vytvoří škálování nastavení s názvem *myScaleSet*:</span><span class="sxs-lookup"><span data-stu-id="df7c9-137">The following example creates a scale set named *myScaleSet*:</span></span>

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
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

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

<span data-ttu-id="df7c9-138">Trvá několik minut vytvořit a nakonfigurovat všechny škálovací sadu prostředků a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="df7c9-138">It takes a few minutes to create and configure all the scale set resources and VMs.</span></span>


## <a name="test-your-app"></a><span data-ttu-id="df7c9-139">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="df7c9-139">Test your app</span></span>
<span data-ttu-id="df7c9-140">Chcete-li zobrazit váš web služby IIS v akci, získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="df7c9-140">To see your IIS website in action, obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="df7c9-141">Následující příklad, získá IP adresu pro *myPublicIP* vytvořené jako součást sady škálování:</span><span class="sxs-lookup"><span data-stu-id="df7c9-141">The following example obtains the IP address for *myPublicIP* created as part of the scale set:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

<span data-ttu-id="df7c9-142">Zadejte veřejnou IP adresu ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="df7c9-142">Enter the public IP address in to a web browser.</span></span> <span data-ttu-id="df7c9-143">Aplikace se zobrazí, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz:</span><span class="sxs-lookup"><span data-stu-id="df7c9-143">The app is displayed, including the hostname of the VM that the load balancer distributed traffic to:</span></span>

![Spuštěné web služby IIS](./media/tutorial-create-vmss/running-iis-site.png)

<span data-ttu-id="df7c9-145">Sad v akci škálování najdete můžete můžete vynutit aktualizujte webový prohlížeč zobrazit nástroje pro vyrovnávání zatížení provoz distribuovat mezi všechny virtuální počítače používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="df7c9-145">To see the scale set in action, you can force-refresh your web browser to see the load balancer distribute traffic across all the VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="df7c9-146">Úlohy správy</span><span class="sxs-lookup"><span data-stu-id="df7c9-146">Management tasks</span></span>
<span data-ttu-id="df7c9-147">V průběhu cyklu škálovací sady můžete spustit jeden nebo více úloh správy.</span><span class="sxs-lookup"><span data-stu-id="df7c9-147">Throughout the lifecycle of the scale set, you may need to run one or more management tasks.</span></span> <span data-ttu-id="df7c9-148">Kromě toho můžete vytvořit skripty, které automatizují různé úlohy životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="df7c9-148">Additionally, you may want to create scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="df7c9-149">Azure PowerShell poskytuje rychlý způsob, jak provést tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="df7c9-149">Azure PowerShell provides a quick way to do those tasks.</span></span> <span data-ttu-id="df7c9-150">Tady jsou několik běžných úloh.</span><span class="sxs-lookup"><span data-stu-id="df7c9-150">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="df7c9-151">Zobrazení virtuální počítače ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="df7c9-151">View VMs in a scale set</span></span>
<span data-ttu-id="df7c9-152">Chcete-li zobrazit seznam virtuálních počítačů spuštěných ve škálovací sadě, použijte [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="df7c9-152">To view a list of VMs running in your scale set, use [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) as follows:</span></span>

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="df7c9-153">Zvýšení nebo snížení instance virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="df7c9-153">Increase or decrease VM instances</span></span>
<span data-ttu-id="df7c9-154">Pokud chcete zobrazit počet instancí, které máte aktuálně v sadě škálování, použijte [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) a dotazovat se na *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="df7c9-154">To see the number of instances you currently have in a scale set, use [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) and query on *sku.capacity*:</span></span>

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

<span data-ttu-id="df7c9-155">Potom můžete ručně zvýšení nebo snížení počtu virtuálních počítačů v sad s škálování [aktualizace AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span><span class="sxs-lookup"><span data-stu-id="df7c9-155">You can then manually increase or decrease the number of virtual machines in the scale set with [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span></span> <span data-ttu-id="df7c9-156">Následující příklad nastaví počet virtuálních počítačů ve vaší škálování nastavena na *5*:</span><span class="sxs-lookup"><span data-stu-id="df7c9-156">The following example sets the number of VMs in your scale set to *5*:</span></span>

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

<span data-ttu-id="df7c9-157">Pokud se používá několik minut se aktualizovat zadaný počet instancí ve vaší škálování nastavit.</span><span class="sxs-lookup"><span data-stu-id="df7c9-157">If takes a few minutes to update the specified number of instances in your scale set.</span></span>


### <a name="configure-autoscale-rules"></a><span data-ttu-id="df7c9-158">Konfigurace pravidel automatického škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-158">Configure autoscale rules</span></span>
<span data-ttu-id="df7c9-159">Místo ručně škálování počtu instancí ve vaší škálovací sady, můžete definovat pravidla automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="df7c9-159">Rather than manually scaling the number of instances in your scale set, you define autoscale rules.</span></span> <span data-ttu-id="df7c9-160">Tato pravidla monitorovat instance ve škálovací sadě a reagují odpovídajícím způsobem na základě metriky a prahové hodnoty, které definujete.</span><span class="sxs-lookup"><span data-stu-id="df7c9-160">These rules monitor the instances in your scale set and respond accordingly based on metrics and thresholds you define.</span></span> <span data-ttu-id="df7c9-161">Následující příklad škáluje se počet instancí jedním při průměrné zatížení procesoru je větší než 60 % během období 5 minut.</span><span class="sxs-lookup"><span data-stu-id="df7c9-161">The following example scales out the number of instances by one when the average CPU load is greater than 60% over a 5 minute period.</span></span> <span data-ttu-id="df7c9-162">Pokud průměrná zatížení procesoru pak klesne pod 30 % během období 5 minut, jsou škálovat instance v jednou instancí:</span><span class="sxs-lookup"><span data-stu-id="df7c9-162">If the average CPU load then drops below 30% over a 5 minute period, the instances are scaled in by one instance:</span></span>

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5 minute period
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

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5 minute period
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

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="next-steps"></a><span data-ttu-id="df7c9-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df7c9-163">Next steps</span></span>
<span data-ttu-id="df7c9-164">V tomto kurzu jste vytvořili škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="df7c9-164">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="df7c9-165">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="df7c9-165">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="df7c9-166">Použijte k definování stránku IIS škálování rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="df7c9-166">Use the Custom Script Extension to define an IIS site to scale</span></span>
> * <span data-ttu-id="df7c9-167">Vytvořit nástroj pro vyrovnávání zatížení pro škálovací sadu</span><span class="sxs-lookup"><span data-stu-id="df7c9-167">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="df7c9-168">Vytvoření sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="df7c9-168">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="df7c9-169">Zvýšení nebo snížení počtu instancí v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-169">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="df7c9-170">Vytvoření pravidel škálování</span><span class="sxs-lookup"><span data-stu-id="df7c9-170">Create autoscale rules</span></span>

<span data-ttu-id="df7c9-171">Přechodu na v dalším kurzu se dozvíte další informace o konceptech pro virtuální počítače Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="df7c9-171">Advance to the next tutorial to learn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="df7c9-172">Vyrovnávat zatížení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="df7c9-172">Load balance virtual machines</span></span>](tutorial-load-balancer.md)
