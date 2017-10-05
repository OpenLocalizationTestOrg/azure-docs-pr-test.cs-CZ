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
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>Vytvoření sady škálování virtuálního počítače pomocí rutin prostředí PowerShell
Tento článek vás provede příklad vytvoření škálovací sadu virtuálních počítačů (VMSS). Vytvoří sadu škálování tři uzly s přidružené sítě a úložiště.

## <a name="first-steps"></a>První kroky
Ujistěte se, že máte nejnovější modul Azure PowerShell nainstalovali, a ujistěte se, že máte rutiny prostředí PowerShell, který je potřeba k údržbě a vytvoření sady škálování.
Přejděte k nástrojům příkazového řádku [sem](http://aka.ms/webpi-azps) pro nejnovější dostupné moduly Azure.

K vyhledání VMSS související rutiny PowerShell, použijte vyhledávací řetězec \*VMSS\*. Například _gcm *vmss*_

## <a name="creating-a-vmss"></a>Vytváření VMSS
#### <a name="create-resource-group"></a>Vytvoření skupiny prostředků
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

### <a name="create-networking-vnet--subnet"></a>Vytvoření sítě (virtuální síť nebo podsíť)
#### <a name="subnet-specification"></a>Specifikace podsítě
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

#### <a name="vnet-specification"></a>Specifikace virtuální sítě
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

# In this case assume the new subnet is the only one
$subnetId = $vnet.Subnets[0].Id;
```

#### <a name="create-public-ip-resource-to-allow-external-access"></a>Vytvořte prostředek veřejné IP adresy povolit externí přístup
To bude vázán ke službě Vyrovnávání zatížení.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

#### <a name="create-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení
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

#### <a name="configure-load-balancer"></a>Konfigurace pro vyrovnávání zatížení
Vytvoření konfigurace fondu adres back-end, to budou sdílet síťové adaptéry virtuálních počítačů v sadě škálování.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Nastavit Port testu vyrovnáváním zatížení, změňte nastavení podle potřeby pro vaši aplikaci.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Vytvořte příchozí fond NAT pro přímé připojení směrované (ne skupiny s vyrovnáváním zatížení) pro virtuální počítače ve škálovací nastavených prostřednictvím nástroje pro vyrovnávání zatížení. Toto je demonstruje použití protokolu RDP a nemusí být potřeba ve vaší aplikace.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Načíst skupinu s vyrovnáváním pravidlo vytvořte, tento příklad ukazuje zátěže vyrovnávání port 80 požadavků, pomocí nastavení z předchozích kroků.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Vytvořte nástroj pro vyrovnávání zatížení s konfigurací.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Zkontrolujte nastavení LB, zkontrolujte konfigurací portů skupinu s vyrovnáváním zatížení, Všimněte si, se nezobrazí pravidla příchozí NAT dokud se vytvoří virtuální počítače v sadě škálování.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-the-scale-set"></a>Konfigurace a vytvořit sadu škálování
Všimněte si, že se tato infrastruktura příklad ukazuje, jak nastavit distribuovat a škálování webových přenosů mezi škálovací sadu, ale bitové kopie virtuálních počítačů, které zde zadané nemají žádné webové služby.

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

Vytvořit vazbu síťový adaptér služby Vyrovnávání zatížení a podsíť

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

Vytvoření škálování nastavení konfigurace

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

Konfigurace sady škálování sestavení

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Nyní jste vytvořili byly sadou škálování. Můžete otestovat připojení k jednotlivých virtuálních počítačů pomocí protokolu RDP v tomto příkladu:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
