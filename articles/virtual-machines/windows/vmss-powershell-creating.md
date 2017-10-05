---
title: "Vytvoření sady škálování virtuálního počítače pomocí rutin prostředí PowerShell | Microsoft Docs"
description: "Začínáme s vytváření a správa vašeho prvního Azure sady škálování virtuálního počítače pomocí rutin prostředí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 430d9d64-1f35-48f0-a4fd-9b69910ffa59
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: danielsollondon
ms.openlocfilehash: a3a36028a75d6cb7eb36277f3e2b5ab833c96a96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a><span data-ttu-id="aad65-103">Vytvoření sady škálování virtuálního počítače pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="aad65-103">Creating Virtual Machine Scale Sets using PowerShell cmdlets</span></span>
<span data-ttu-id="aad65-104">Tento článek vás provede příklad vytvoření škálovací sadu virtuálních počítačů (VMSS).</span><span class="sxs-lookup"><span data-stu-id="aad65-104">This article walks through an example of how to create a Virtual Machine scale set (VMSS).</span></span> <span data-ttu-id="aad65-105">Vytvoří sadu škálování tři uzly s přidružené sítě a úložiště.</span><span class="sxs-lookup"><span data-stu-id="aad65-105">It creates a scale set of three nodes, with associated Networking and Storage.</span></span>

## <a name="first-steps"></a><span data-ttu-id="aad65-106">První kroky</span><span class="sxs-lookup"><span data-stu-id="aad65-106">First Steps</span></span>
<span data-ttu-id="aad65-107">Ujistěte se, že máte nejnovější modul Azure PowerShell nainstalovali, a ujistěte se, že máte rutiny prostředí PowerShell, který je potřeba k údržbě a vytvoření sady škálování.</span><span class="sxs-lookup"><span data-stu-id="aad65-107">Ensure you have the latest Azure PowerShell module installed, to make sure you have the PowerShell commandlets needed to maintain, and create scale sets.</span></span>
<span data-ttu-id="aad65-108">Přejděte k nástrojům příkazového řádku [sem](http://aka.ms/webpi-azps) pro nejnovější dostupné moduly Azure.</span><span class="sxs-lookup"><span data-stu-id="aad65-108">Go to the command line tools [here](http://aka.ms/webpi-azps) for the latest available Azure Modules.</span></span>

<span data-ttu-id="aad65-109">K vyhledání VMSS související rutiny PowerShell, použijte vyhledávací řetězec \*VMSS\*.</span><span class="sxs-lookup"><span data-stu-id="aad65-109">To find VMSS related commandlets, use the search string \*VMSS\*.</span></span> <span data-ttu-id="aad65-110">Například _gcm *vmss*_</span><span class="sxs-lookup"><span data-stu-id="aad65-110">For example, _gcm *vmss*_</span></span>

## <a name="creating-a-vmss"></a><span data-ttu-id="aad65-111">Vytváření VMSS</span><span class="sxs-lookup"><span data-stu-id="aad65-111">Creating a VMSS</span></span>
#### <a name="create-resource-group"></a><span data-ttu-id="aad65-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="aad65-112">Create Resource Group</span></span>
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

### <a name="create-networking-vnet--subnet"></a><span data-ttu-id="aad65-113">Vytvoření sítě (virtuální síť nebo podsíť)</span><span class="sxs-lookup"><span data-stu-id="aad65-113">Create Networking (VNET / Subnet)</span></span>
#### <a name="subnet-specification"></a><span data-ttu-id="aad65-114">Specifikace podsítě</span><span class="sxs-lookup"><span data-stu-id="aad65-114">Subnet Specification</span></span>
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

#### <a name="vnet-specification"></a><span data-ttu-id="aad65-115">Specifikace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="aad65-115">VNET Specification</span></span>
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

# In this case assume the new subnet is the only one
$subnetId = $vnet.Subnets[0].Id;
```

#### <a name="create-public-ip-resource-to-allow-external-access"></a><span data-ttu-id="aad65-116">Vytvořte prostředek veřejné IP adresy povolit externí přístup</span><span class="sxs-lookup"><span data-stu-id="aad65-116">Create Public IP Resource to Allow External Access</span></span>
<span data-ttu-id="aad65-117">To bude vázán ke službě Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="aad65-117">This will be bound to the Load Balancer.</span></span>

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

#### <a name="create-load-balancer"></a><span data-ttu-id="aad65-118">Vytvořit nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="aad65-118">Create Load Balancer</span></span>
```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

# Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

#### <a name="configure-load-balancer"></a><span data-ttu-id="aad65-119">Konfigurace pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="aad65-119">Configure Load Balancer</span></span>
<span data-ttu-id="aad65-120">Vytvoření konfigurace fondu adres back-end, to budou sdílet síťové adaptéry virtuálních počítačů v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="aad65-120">Create Backend Address Pool Config, this will be shared by the NICs of the VMs in the scale set.</span></span>

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

<span data-ttu-id="aad65-121">Nastavit Port testu vyrovnáváním zatížení, změňte nastavení podle potřeby pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aad65-121">Set Load Balanced Probe Port, change the settings as appropriate for your application.</span></span>

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

<span data-ttu-id="aad65-122">Vytvořte příchozí fond NAT pro přímé připojení směrované (ne skupiny s vyrovnáváním zatížení) pro virtuální počítače ve škálovací nastavených prostřednictvím nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="aad65-122">Create an inbound NAT pool for direct routed connectivity (not load balanced) to the VMs in the scale set via the Load Balancer.</span></span> <span data-ttu-id="aad65-123">Toto je demonstruje použití protokolu RDP a nemusí být potřeba ve vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="aad65-123">This is to demonstrate using RDP and may not be required in your application.</span></span>

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

<span data-ttu-id="aad65-124">Načíst skupinu s vyrovnáváním pravidlo vytvořte, tento příklad ukazuje zátěže vyrovnávání port 80 požadavků, pomocí nastavení z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="aad65-124">Create the Load Balanced Rule, this example shows load balancing port 80 requests, using the settings from previous steps.</span></span>

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

<span data-ttu-id="aad65-125">Vytvořte nástroj pro vyrovnávání zatížení s konfigurací.</span><span class="sxs-lookup"><span data-stu-id="aad65-125">Create Load Balancer with configuration.</span></span>

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

<span data-ttu-id="aad65-126">Zkontrolujte nastavení LB, zkontrolujte konfigurací portů skupinu s vyrovnáváním zatížení, Všimněte si, se nezobrazí pravidla příchozí NAT dokud se vytvoří virtuální počítače v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="aad65-126">Check  LB settings, check load balanced port configs, note, you will not see Inbound NAT rules until the VMs in the scale set are created.</span></span>

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-the-scale-set"></a><span data-ttu-id="aad65-127">Konfigurace a vytvořit sadu škálování</span><span class="sxs-lookup"><span data-stu-id="aad65-127">Configure and Create the scale set</span></span>
<span data-ttu-id="aad65-128">Všimněte si, že se tato infrastruktura příklad ukazuje, jak nastavit distribuovat a škálování webových přenosů mezi škálovací sadu, ale bitové kopie virtuálních počítačů, které zde zadané nemají žádné webové služby.</span><span class="sxs-lookup"><span data-stu-id="aad65-128">Note, this infrastructure example shows how to set up distribute and scale web traffic across the scale set, but the VMs Images specified here do not have any web services installed.</span></span>

```
# specify scale set Name
$vmssName = 'vmss' + $rgname;

## specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

<span data-ttu-id="aad65-129">Vytvořit vazbu síťový adaptér služby Vyrovnávání zatížení a podsíť</span><span class="sxs-lookup"><span data-stu-id="aad65-129">Bind NIC to Load Balancer and Subnet</span></span>

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

<span data-ttu-id="aad65-130">Vytvoření škálování nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="aad65-130">Create scale set Config</span></span>

```
# Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
    | Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
    | Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
    | Set-AzureRmVmssStorageProfile -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName `
    | Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

<span data-ttu-id="aad65-131">Konfigurace sady škálování sestavení</span><span class="sxs-lookup"><span data-stu-id="aad65-131">Build scale set configuration</span></span>

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

<span data-ttu-id="aad65-132">Nyní jste vytvořili byly sadou škálování.</span><span class="sxs-lookup"><span data-stu-id="aad65-132">Now you have created the scale set.</span></span> <span data-ttu-id="aad65-133">Můžete otestovat připojení k jednotlivých virtuálních počítačů pomocí protokolu RDP v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="aad65-133">You can test connecting to the individual VM using RDP in this example:</span></span>

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
