---
title: "Nastaví aaaCreating škálování virtuálních počítačů pomocí rutin prostředí PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 7979be367d04c904b60d78849c1b751a52cc8caf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a><span data-ttu-id="2dca1-103">Vytvoření sady škálování virtuálního počítače pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2dca1-103">Creating Virtual Machine Scale Sets using PowerShell cmdlets</span></span>
<span data-ttu-id="2dca1-104">Tento článek vás provede příklady jak toocreate škálovací sady virtuálních počítačů (VMSS).</span><span class="sxs-lookup"><span data-stu-id="2dca1-104">This article walks through an example of how toocreate a Virtual Machine scale set (VMSS).</span></span> <span data-ttu-id="2dca1-105">Vytvoří sadu škálování tři uzly s přidružené sítě a úložiště.</span><span class="sxs-lookup"><span data-stu-id="2dca1-105">It creates a scale set of three nodes, with associated Networking and Storage.</span></span>

## <a name="first-steps"></a><span data-ttu-id="2dca1-106">První kroky</span><span class="sxs-lookup"><span data-stu-id="2dca1-106">First Steps</span></span>
<span data-ttu-id="2dca1-107">Ujistěte se, máte nejnovější modul Azure PowerShell hello nainstalovaný, toomake se, že máte rutiny prostředí PowerShell hello potřeby toomaintain a vytvoření sady škálování.</span><span class="sxs-lookup"><span data-stu-id="2dca1-107">Ensure you have hello latest Azure PowerShell module installed, toomake sure you have hello PowerShell commandlets needed toomaintain, and create scale sets.</span></span>
<span data-ttu-id="2dca1-108">Přejděte nástroje příkazového řádku toohello [sem](http://aka.ms/webpi-azps) pro hello nejnovější dostupné moduly Azure.</span><span class="sxs-lookup"><span data-stu-id="2dca1-108">Go toohello command line tools [here](http://aka.ms/webpi-azps) for hello latest available Azure Modules.</span></span>

<span data-ttu-id="2dca1-109">toofind VMSS související rutiny PowerShell, použijte vyhledávací řetězec hello \*VMSS\*.</span><span class="sxs-lookup"><span data-stu-id="2dca1-109">toofind VMSS related commandlets, use hello search string \*VMSS\*.</span></span> <span data-ttu-id="2dca1-110">Například _gcm *vmss*_</span><span class="sxs-lookup"><span data-stu-id="2dca1-110">For example, _gcm *vmss*_</span></span>

## <a name="creating-a-vmss"></a><span data-ttu-id="2dca1-111">Vytváření VMSS</span><span class="sxs-lookup"><span data-stu-id="2dca1-111">Creating a VMSS</span></span>
#### <a name="create-resource-group"></a><span data-ttu-id="2dca1-112">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="2dca1-112">Create Resource Group</span></span>
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

### <a name="create-networking-vnet--subnet"></a><span data-ttu-id="2dca1-113">Vytvoření sítě (virtuální síť nebo podsíť)</span><span class="sxs-lookup"><span data-stu-id="2dca1-113">Create Networking (VNET / Subnet)</span></span>
#### <a name="subnet-specification"></a><span data-ttu-id="2dca1-114">Specifikace podsítě</span><span class="sxs-lookup"><span data-stu-id="2dca1-114">Subnet Specification</span></span>
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

#### <a name="vnet-specification"></a><span data-ttu-id="2dca1-115">Specifikace virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2dca1-115">VNET Specification</span></span>
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

# In this case assume hello new subnet is hello only one
$subnetId = $vnet.Subnets[0].Id;
```

#### <a name="create-public-ip-resource-tooallow-external-access"></a><span data-ttu-id="2dca1-116">Vytvořte prostředek veřejné IP tooAllow externí přístup</span><span class="sxs-lookup"><span data-stu-id="2dca1-116">Create Public IP Resource tooAllow External Access</span></span>
<span data-ttu-id="2dca1-117">Bude jím vázané toohello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2dca1-117">This will be bound toohello Load Balancer.</span></span>

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

#### <a name="create-load-balancer"></a><span data-ttu-id="2dca1-118">Vytvořit nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2dca1-118">Create Load Balancer</span></span>
```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

# Bind Public IP tooLoad Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

#### <a name="configure-load-balancer"></a><span data-ttu-id="2dca1-119">Konfigurace pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2dca1-119">Configure Load Balancer</span></span>
<span data-ttu-id="2dca1-120">Vytvoření konfigurace fondu adres back-end, to budou sdílet hello síťové adaptéry virtuálních počítačů hello v sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="2dca1-120">Create Backend Address Pool Config, this will be shared by hello NICs of hello VMs in hello scale set.</span></span>

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

<span data-ttu-id="2dca1-121">Nastavit Port testu vyrovnáváním zatížení, změňte nastavení hello podle potřeby pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2dca1-121">Set Load Balanced Probe Port, change hello settings as appropriate for your application.</span></span>

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

<span data-ttu-id="2dca1-122">Vytvořte příchozí fond NAT pro přímé připojení směrované (ne skupiny s vyrovnáváním zatížení) toohello virtuální počítače ve škálovací hello nastavených prostřednictvím hello Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="2dca1-122">Create an inbound NAT pool for direct routed connectivity (not load balanced) toohello VMs in hello scale set via hello Load Balancer.</span></span> <span data-ttu-id="2dca1-123">Toto je toodemonstrate pomocí protokolu RDP a nemusí být potřeba ve vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2dca1-123">This is toodemonstrate using RDP and may not be required in your application.</span></span>

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

<span data-ttu-id="2dca1-124">Vytvořte hello načíst skupinu s vyrovnáváním pravidlo tento příklad ukazuje zátěže vyrovnávání port 80 požadavků, pomocí nastavení hello z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="2dca1-124">Create hello Load Balanced Rule, this example shows load balancing port 80 requests, using hello settings from previous steps.</span></span>

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

<span data-ttu-id="2dca1-125">Vytvořte nástroj pro vyrovnávání zatížení s konfigurací.</span><span class="sxs-lookup"><span data-stu-id="2dca1-125">Create Load Balancer with configuration.</span></span>

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

<span data-ttu-id="2dca1-126">Zkontrolujte nastavení LB, zkontrolujte zatížení vyrovnáváním port konfigurací, Všimněte si, že se nezobrazí, vytvoří příchozí NAT pravidla, dokud hello virtuální počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="2dca1-126">Check  LB settings, check load balanced port configs, note, you will not see Inbound NAT rules until hello VMs in hello scale set are created.</span></span>

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-hello-scale-set"></a><span data-ttu-id="2dca1-127">Nakonfigurujte a nastavte vytvořit hello škálování</span><span class="sxs-lookup"><span data-stu-id="2dca1-127">Configure and Create hello scale set</span></span>
<span data-ttu-id="2dca1-128">Všimněte si, že se tento infrastruktury příklad ukazuje, jak distribuovat tooset nahoru a škálování webových přenosů mezi hello škálovací sadu, ale hello bitové kopie virtuálních počítačů, které zde zadané nemají žádné webové služby.</span><span class="sxs-lookup"><span data-stu-id="2dca1-128">Note, this infrastructure example shows how tooset up distribute and scale web traffic across hello scale set, but hello VMs Images specified here do not have any web services installed.</span></span>

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

<span data-ttu-id="2dca1-129">Vytvoření vazby tooLoad síťový adaptér služby Vyrovnávání a podsíť</span><span class="sxs-lookup"><span data-stu-id="2dca1-129">Bind NIC tooLoad Balancer and Subnet</span></span>

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

<span data-ttu-id="2dca1-130">Vytvoření škálování nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="2dca1-130">Create scale set Config</span></span>

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

<span data-ttu-id="2dca1-131">Konfigurace sady škálování sestavení</span><span class="sxs-lookup"><span data-stu-id="2dca1-131">Build scale set configuration</span></span>

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

<span data-ttu-id="2dca1-132">Nyní jste vytvořili hello škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="2dca1-132">Now you have created hello scale set.</span></span> <span data-ttu-id="2dca1-133">Můžete otestovat připojování toohello jednotlivých virtuálních počítačů pomocí protokolu RDP v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="2dca1-133">You can test connecting toohello individual VM using RDP in this example:</span></span>

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
