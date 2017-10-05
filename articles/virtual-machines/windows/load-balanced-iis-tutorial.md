---
title: "Kurz – sestavení vysokou dostupnost aplikací na virtuálních počítačích Azure | Microsoft Docs"
description: "Naučte se vytvořit vysoce dostupné a zabezpečení aplikací v rámci tři virtuální počítače Windows se nástroj pro vyrovnávání zatížení v Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/30/2017
ms.author: davidmu
ms.openlocfilehash: 4b8690a11ec0e711782a112622e1193c24292289
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a>Sestavení zatížení vyrovnáváním, vysokou dostupnost aplikací na virtuálních počítačích s Windows v Azure

V tomto kurzu vytvoříte vysoce dostupné aplikace, která je odolné vůči události údržby. Aplikace používá nástroj pro vyrovnávání zatížení, skupinu dostupnosti a tři Windows virtuální počítače (VM). V tomto kurzu nainstaluje službu IIS, i když v tomto kurzu můžete použít k nasazení jinou aplikaci rozhraní pomocí stejné komponenty vysokou dostupnost a pokyny. 

## <a name="step-1---azure-prerequisites"></a>Krok 1 – požadavky Azure

K dokončení tohoto kurzu, ujistěte se, že jste nainstalovali nejnovější [prostředí Azure PowerShell](/powershell/azure/overview) modulu.

První, přihlaste se k předplatnému Azure pomocí příkazu Login-AzureRmAccount a postupujte podle na obrazovce pokynů.

```powershell
Login-AzureRmAccount
```

Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před vytvořením veškeré prostředky Azure, je nutné vytvořit skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `westeurope` oblasti: 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a>Krok 2 – Vytvoření sady dostupnosti.

Virtuální počítače lze vytvořit v logické chyby a aktualizaci domény. Každé logické domény představuje část hardwaru v datovém centru základní Azure. Když vytvoříte dva nebo více virtuálních počítačů, výpočetní a úložnou kapacitu jsou rozmístěny v těchto doménách. Tento distribuční udržuje dostupnost aplikace, pokud hardwarová komponenta potřebuje údržby. Skupiny dostupnosti umožňují definovat tyto logické domény selhání a aktualizace.

Vytvořit sadu s dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a>Krok 3 – vytvoření zatížení vyrovnávání

K nástroji pro vyrovnávání zatížení Azure rozděluje zatížení mezi sadu definované virtuálních počítačů pomocí pravidla nástroje pro vyrovnávání zatížení. Test stavu monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom přenosy na provozní virtuální počítač.

### <a name="create-public-ip-address"></a>Vytvoření veřejné IP adresy

Pro přístup k vaší aplikace na Internetu, přiřaďte veřejnou IP adresu nástroji pro vyrovnávání zatížení. Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP`:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení

Vytvoření front-endovou IP adresy s [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Následující příklad vytvoří na front-endovou IP adresu s názvem `myFrontEndPool`: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

Vytvořit fond adres back-end s [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). Následující příklad vytvoří fond back-end adresy s názvem `myBackEndPool`:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Nyní, vytvoří se službou Vyrovnávání zatížení s [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). Následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer` pomocí `myPublicIP` adresa:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a>Vytvoření test stavu

Povolit službu Vyrovnávání zatížení k monitorování stavu aplikace, použijte Test stavu. Test stavu dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení, podle jejich reakce na kontroly stavu. Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení po dvě po sobě jdoucích selhání v intervalech 15 sekund.

Vytvoření test stavu s [přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Následující příklad vytvoří kontrolu stavu s názvem `myHealthProbe` který monitoruje každý virtuální počítač:

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a>Vytvořit pravidlo Vyrovnávání zatížení.

Pravidlo Vyrovnávání zatížení se používá k definování, jak se provoz rozděluje k virtuálním počítačům.

Vytvořit pravidlo Vyrovnávání zatížení s [přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). Následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem `myLoadBalancerRule` a vyrovnává přenosy na portu `80`:

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

Aktualizace se službou Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a>Krok 4 – konfigurace sítí

Každý virtuální počítač má jeden nebo více virtuálních síťových karet (NIC) připojených k virtuální síti. Tato virtuální síť, je zabezpečena k filtrování provozu založené na definovaných pravidel přístupu.

### <a name="create-virtual-network"></a>Vytvoření virtuální sítě

Nejprve nakonfigurujte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Následující příklad vytvoří podsíť s názvem `mySubnet`:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

Chcete-li poskytovat připojení k síti virtuálních počítačů, vytvořte virtuální síť s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Následující příklad vytvoří virtuální síť s názvem `myVnet` s `mySubnet`:

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a>Konfigurace zabezpečení sítě

Azure [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) (NSG) řídí příchozí a odchozí přenosy pro jeden nebo více virtuálních počítačů. Pravidla skupiny zabezpečení sítě, povolit nebo odepřít síťový provoz na konkrétní port nebo rozsah portů. Tato pravidla může zahrnovat předpona zdrojové adresy tak, aby jenom přenosy v předdefinované zdroj může komunikovat s virtuálním počítačem.

Povolit webový provoz k dosažení vaší aplikace, vytvořte pravidlo skupiny zabezpečení sítě s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem `myNetworkSecurityGroupRule`:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

Vytvořit skupinu zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Následující příklad vytvoří skupinu NSG s názvem `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

Přidat skupinu zabezpečení sítě pro podsíť s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

Aktualizace virtuální sítě s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a>Vytvořit virtuální síťové karty

Načíst funkce Vyrovnávání prostředků virtuální síťový adaptér, nikoli skutečné virtuálního počítače. Virtuální síťový adaptér je připojený ke službě Vyrovnávání zatížení a potom připojen k virtuálnímu počítači.

Vytvořit virtuální síťovou kartu s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Následující příklad vytvoří tři virtuálních síťových karet. (Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro vaši aplikaci v následujících krocích):


```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroup `
     -Name myNic$i `
     -Location westeurope `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}

```

## <a name="step-5---create-virtual-machines"></a>Krok 5 – vytvoření virtuálních počítačů

S všechny základní součásti v místě nyní můžete vytvořit vysoce dostupné virtuální počítače ke spouštění vaší aplikace. 

Uživatelské jméno a heslo, které potřebuje pro účet správce na virtuálním počítači s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Vytvoření virtuálních počítačů s [nové AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [přidat AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Následující příklad vytvoří tři virtuální počítače:

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -StorageAccountType StandardLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vm
}

```

Trvá několik minut vytvořit a nakonfigurovat všechny tři virtuální počítače. Test stavu nástroje pro vyrovnávání zatížení automaticky rozpozná, když aplikace běží na každém virtuálním počítači. Jakmile aplikace běží, spustí se pravidlo Vyrovnávání zatížení k distribuci přenosů.

### <a name="install-the-app"></a>Nainstalujte aplikaci 

Rozšíření virtuálního počítače Azure se používají k automatizaci úloh konfigurace virtuálního počítače jako je instalace aplikací a konfigurace operačního systému. [Rozšíření vlastních skriptů pro systém Windows](./../virtual-machines-windows-extensions-customscript.md) se používá ke spuštění všech skriptů prostředí PowerShell na virtuálním počítači. Skript může uložena v úložišti Azure, žádný dostupný koncový bod HTTP nebo vložené v konfiguraci rozšíření vlastních skriptů. Pokud používáte rozšíření vlastních skriptů, agent virtuálního počítače Azure spravuje provádění skriptu.

Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) k instalaci rozšíření vlastních skriptů. Spustí rozšíření `powershell Add-WindowsFeature Web-Server` nainstalovat webový server služby IIS:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
     -Location westeurope
}
```

### <a name="test-your-app"></a>Testování aplikace

Získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Následující příklad, získá IP adresu pro `myPublicIP` vytvořili dříve:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

Zadejte veřejnou IP adresu ve webovém prohlížeči. Pomocí pravidel NSG na místě se zobrazí výchozí web služby IIS. 

![Výchozí web služby IIS](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a>Krok 6 – úlohy správy

Potřebujete provést údržbu na virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému. Jak nakládat s zvýšení provozu do vaší aplikace, musíte pro přidání dalších virtuálních počítačů. V této části se dozvíte, jak odebrat nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení. 

### <a name="remove-a-vm-from-the-load-balancer"></a>Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení

Virtuální počítač odeberte z fondu adres back-end resetováním vlastnost pravidlo LoadBalancerBackendAddressPools karty síťového rozhraní.

Získat karty síťového rozhraní s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

Nastaví vlastnost pravidlo LoadBalancerBackendAddressPools karty síťového rozhraní na $null:

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

Aktualizace síťovou kartu:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-to-the-load-balancer"></a>Přidat virtuální počítač ke službě Vyrovnávání zatížení

Po provedení údržby virtuálních počítačů, nebo pokud potřebujete rozšířit kapacitu, přidání síťový adaptér virtuálního počítače do fondu back-end adresy služby Vyrovnávání zatížení.

Získáte nástroje pro vyrovnávání zatížení:

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

Back-endových adres nástroje pro vyrovnávání zatížení přidáte do karty síťového rozhraní:

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

Aktualizace síťovou kartu:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Další kroky

Ukázky – [ukázkové skripty prostředí Azure PowerShell virtuálního počítače](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
