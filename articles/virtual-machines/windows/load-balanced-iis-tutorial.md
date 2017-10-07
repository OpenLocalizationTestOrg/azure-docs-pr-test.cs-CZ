---
title: "aaaTutorial - sestavení vysokou dostupnost aplikací na virtuálních počítačích Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate vysoká dostupnost a zabezpečení aplikací mezi tři virtuální počítače Windows se nástroj pro vyrovnávání zatížení v Azure"
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
ms.openlocfilehash: f9eff96be4f3999651c4108f0334e4eaa1a39c0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a>Sestavení zatížení vyrovnáváním, vysokou dostupnost aplikací na virtuálních počítačích s Windows v Azure

V tomto kurzu vytvoříte vysoce dostupné aplikace, která je odolný toomaintenance události. aplikace Hello používá nástroj pro vyrovnávání zatížení, skupinu dostupnosti a tři Windows virtuální počítače (VM). V tomto kurzu nainstaluje službu IIS, i když můžete použít tento kurz toodeploy framework jinou aplikaci pomocí hello stejné komponenty vysokou dostupnost a pokyny. 

## <a name="step-1---azure-prerequisites"></a>Krok 1 – požadavky Azure

toocomplete tohoto kurzu, ujistěte se, že jste nainstalovali hello nejnovější [prostředí Azure PowerShell](/powershell/azure/overview) modulu.

Nejdřív přihlásit tooyour předplatné s hello příkaz Login-AzureRmAccount a postupujte podle hello na obrazovce pokynů.

```powershell
Login-AzureRmAccount
```

Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před vytvořením veškeré prostředky Azure, je nutné toocreate skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westeurope` oblasti: 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a>Krok 2 – Vytvoření sady dostupnosti.

Virtuální počítače lze vytvořit v logické chyby a aktualizaci domény. Každé logické domény představuje část hardwaru v datovém základní Azure hello. Když vytvoříte dva nebo více virtuálních počítačů, výpočetní a úložnou kapacitu jsou rozmístěny v těchto doménách. Tento distribuční udržuje hello dostupnosti vaší aplikace, pokud hardwarová komponenta potřebuje údržby. Skupiny dostupnosti umožňují definovat tyto logické domény selhání a aktualizace.

Vytvořit sadu s dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Hello následující příklad vytvoří sadu s názvem dostupnosti `myAvailabilitySet`:

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

K nástroji pro vyrovnávání zatížení Azure rozděluje zatížení mezi sadu definované virtuálních počítačů pomocí pravidla nástroje pro vyrovnávání zatížení. Test stavu monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom provoz tooan provozní virtuálních počítačů.

### <a name="create-public-ip-address"></a>Vytvoření veřejné IP adresy

tooaccess vaši aplikaci ve hello Internetu, přiřaďte veřejnou IP adresu toohello služby Vyrovnávání zatížení. Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Hello následující příklad vytvoří veřejnou IP adresu s názvem `myPublicIP`:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení

Vytvoření front-endovou IP adresy s [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Hello následující příklad vytvoří na front-endovou IP adresu s názvem `myFrontEndPool`: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

Vytvořit fond adres back-end s [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). Hello následující příklad vytvoří fond back-end adresy s názvem `myBackEndPool`:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Teď vytvořte hello Vyrovnávání zatížení s [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem `myLoadBalancer` pomocí hello `myPublicIP` adresa:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a>Vytvoření test stavu

tooallow hello stavu služby Vyrovnávání zatížení toomonitor hello vaší aplikace, použijte Test stavu. Test stavu Hello dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení hello podle jejich kontroly toohealth odpovědi. Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení hello po dvě po sobě jdoucích selhání v intervalech 15 sekund.

Vytvoření test stavu s [přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Hello následující příklad vytvoří sondu stavu s názvem `myHealthProbe` který monitoruje každý virtuální počítač:

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a>Vytvořit pravidlo Vyrovnávání zatížení.

Pravidlo Vyrovnávání zatížení je použité toodefine jak přenosy jsou distribuované toohello virtuálních počítačů.

Vytvořit pravidlo Vyrovnávání zatížení s [přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). Hello následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem `myLoadBalancerRule` a vyrovnává přenosy na portu `80`:

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

Aktualizovat hello Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a>Krok 4 – konfigurace sítí

Každý virtuální počítač má jeden nebo více virtuálních síťových karet (NIC) připojujících tooa virtuální sítě. Tato virtuální síť, je zabezpečena toofilter provozu založené na definovaných pravidel přístupu.

### <a name="create-virtual-network"></a>Vytvoření virtuální sítě

Nejprve nakonfigurujte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Hello následující příklad vytvoří podsíť s názvem `mySubnet`:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

tooprovide síťové připojení tooyour virtuální počítače, vytvořte virtuální síť s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Hello následující příklad vytvoří virtuální síť s názvem `myVnet` s `mySubnet`:

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

tooallow tooreach provoz webové aplikace, vytvořte pravidlo skupiny zabezpečení sítě s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Hello následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem `myNetworkSecurityGroupRule`:

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

Vytvořit skupinu zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Hello následující příklad vytvoří skupinu NSG s názvem `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

Přidat hello toohello podsíť skupiny zabezpečení sítě s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

Aktualizace hello virtuální síť s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a>Vytvořit virtuální síťové karty

Funkce hello virtuální síťový adaptér prostředků nástroje pro vyrovnávání zatížení a nikoli hello skutečné virtuálních počítačů. Hello virtuální síťovou kartu připojené toohello služba Vyrovnávání zatížení a pak připojit tooa virtuálních počítačů.

Vytvořit virtuální síťovou kartu s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Hello následující příklad vytvoří tři virtuálních síťových karet. (Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro aplikace v rámci hello následující kroky):


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

S všechny hello základní součásti v místě nyní můžete vytvořit vysoce dostupné virtuální počítače toorun vaší aplikace. 

Získat hello uživatelské jméno a heslo, které potřebuje pro účet správce hello na hello virtuální počítač s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Vytvoření hello virtuálních počítačů s [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Přidat AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), a [nové AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Hello následující ukázka vytvoří tři virtuální počítače:

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

Trvá několik minut toocreate a nakonfigurovat všechny tři virtuální počítače. Hello test stavu nástroje pro vyrovnávání zatížení automaticky zjišťuje, když na každém virtuálním počítači běží aplikace hello. Jakmile hello aplikace běží, se spustí pravidlo Vyrovnávání zatížení hello toodistribute provoz.

### <a name="install-hello-app"></a>Instalace aplikace hello 

Rozšíření virtuálního počítače Azure jsou úlohy konfigurace virtuálního počítače používané tooautomate jako je instalace aplikací a konfigurace hello operačního systému. Hello [rozšíření vlastních skriptů pro systém Windows](./../virtual-machines-windows-extensions-customscript.md) je použité toorun všech skriptů prostředí PowerShell na virtuálním počítači hello. Hello skriptu můžete uložené v úložišti Azure, žádný dostupný koncový bod HTTP nebo vložené v konfiguraci rozšíření vlastních skriptů hello. Pokud používáte rozšíření vlastních skriptů hello, spravuje agenta virtuálního počítače Azure hello hello provádění skriptu.

Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) rozšíření vlastních skriptů tooinstall hello. Hello rozšíření spustí `powershell Add-WindowsFeature Web-Server` webový server IIS tooinstall hello:

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

Získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Hello následující příklad získá hello IP adresu pro `myPublicIP` vytvořili dříve:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

Zadejte ve webovém prohlížeči tooa hello veřejnou IP adresu. S hello pravidla NSG v místě se zobrazí výchozí web služby IIS hello. 

![Výchozí web služby IIS](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a>Krok 6 – úlohy správy

Může být nutné tooperform údržby na hello virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému. toodeal zvýšení provozu tooyour aplikace, může být nutné tooadd dalších virtuálních počítačů. V této části se dozvíte, jak tooremove nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení hello. 

### <a name="remove-a-vm-from-hello-load-balancer"></a>Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení hello

Virtuální počítač odeberte z fondu adres back-end hello resetováním hello pravidlo LoadBalancerBackendAddressPools vlastnost hello karty síťového rozhraní.

Získat hello síťová karta s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

Nastaví vlastnost pravidlo LoadBalancerBackendAddressPools hello hello síťová karta příliš$ null:

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

Aktualizace hello síťovou kartu:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-toohello-load-balancer"></a>Přidání toohello virtuálního počítače, služby pro vyrovnávání zatížení

Po provedení údržby virtuálních počítačů, nebo pokud potřebujete tooexpand kapacitu, přidání hello síťový adaptér virtuálních počítačů toohello back-end fondu adres nástroje pro vyrovnávání zatížení hello.

Získejte nástroj pro vyrovnávání zatížení hello:

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

Přidejte fond adres back-end hello hello zatížení vyrovnávání toohello síťovou kartu:

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

Aktualizace hello síťovou kartu:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Další kroky

Ukázky – [ukázkové skripty prostředí Azure PowerShell virtuálního počítače](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
