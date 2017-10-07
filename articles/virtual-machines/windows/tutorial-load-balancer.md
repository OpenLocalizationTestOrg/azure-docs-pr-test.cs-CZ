---
title: "aaaHow tooload vyvážit virtuálních počítačích s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure zatížení vyrovnávání toocreate vysoká dostupnost a zabezpečení aplikací v rámci tři virtuální počítače Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bfbd6a24e90a33e60d7d4f7b0c471af5d4e71f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-windows-virtual-machines-in-azure-toocreate-a-highly-available-application"></a>Jak tooload vyvážit virtuální počítače s Windows v Azure toocreate vysoce dostupné aplikace
Vyrovnávání zatížení poskytuje vyšší úroveň dostupnosti rozloží příchozí žádosti napříč více virtuálních počítačů. V tomto kurzu informace o hello různé součásti nástroje pro vyrovnávání zatížení Azure hello které distribuci přenosů a zajištění vysoké dostupnosti. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření pro vyrovnávání zatížení Azure
> * Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
> * Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení
> * Použít hello rozšíření vlastních skriptů toocreate základní webu IIS
> * Vytváření virtuálních počítačů a připojte tooa nástroj pro vyrovnávání zatížení
> * Zobrazit nástroj pro vyrovnávání zatížení v akci
> * Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="azure-load-balancer-overview"></a>Přehled nástroje pro vyrovnávání zatížení Azure
K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku. Stav sondu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a pouze distribuuje provoz tooan provozní virtuálních počítačů.

Můžete definovat na front-endové konfiguraci protokolu IP, která obsahuje jeden nebo více veřejné IP adresy. Tuto konfiguraci front-end IP adresy umožňuje vaší zatížení vyrovnávání a aplikace toobe přístupné přes hello Internet. 

Virtuální počítače připojit nástroj pro vyrovnávání zatížení tooa pomocí jejich virtuální síťová karta (NIC). toodistribute toohello přenosy virtuálních počítačů, fond back-end adres obsahuje hello IP adresy služby Vyrovnávání zatížení připojená toohello virtuální (NIC) hello.

toocontrol hello tok přenosů, můžete definovat pravidla nástroje pro vyrovnávání zatížení pro určité porty a protokoly, které mapují tooyour virtuálních počítačů.


## <a name="create-azure-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení Azure
V této části podrobně popisuje, jak můžete vytvořit a nakonfigurovat jednotlivé komponenty služby Vyrovnávání zatížení hello. Než bude možné vytvořit nástroj pro vyrovnávání zatížení, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupLoadBalancer* v hello *EastUS* umístění:

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy
tooaccess hello vaší aplikace na Internetu, musíte na veřejnou IP adresu pro nástroj pro vyrovnávání zatížení hello. Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Hello následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* v hello *myResourceGroupLoadBalancer* skupiny prostředků:

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a>Vytvoření nástroje pro vyrovnávání zatížení
Vytvoření front-endovou IP adresy s [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Hello následující příklad vytvoří na front-endovou IP adresu s názvem *myFrontEndPool*: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

Vytvořit fond adres back-end s [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). Hello následující příklad vytvoří fond back-end adresy s názvem *myBackEndPool*:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Teď vytvořte hello Vyrovnávání zatížení s [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem *myLoadBalancer* pomocí hello *myPublicIP* adresa:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Vytvoření test stavu
tooallow hello stavu služby Vyrovnávání zatížení toomonitor hello vaší aplikace, použijte Test stavu. Test stavu Hello dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení hello podle jejich kontroly toohealth odpovědi. Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení hello po dvě po sobě jdoucích selhání v intervalech 15 sekund. Můžete vytvořit test stavu na základě protokolu nebo na stránce Kontrola specifickém stavu pro vaši aplikaci. 

Hello následující ukázka vytvoří sondou TCP. Můžete také vytvořit vlastní sondy HTTP pro další kontroly podrobné stavu. Pokud používáte vlastní sondu HTTP, musíte vytvořit hello stránka pro kontrolu stavu, jako například *healthcheck.aspx*. Test Hello musí vrátit **HTTP 200 OK** odpovědi pro hello zatížení vyrovnávání tookeep hello hostitele v otočení.

toocreate sondou TCP stavu, můžete použít [přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Hello následující příklad vytvoří sondu stavu s názvem *myHealthProbe* který monitoruje každý virtuální počítač:

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Aktualizovat hello Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Vytvořit pravidlo Vyrovnávání zatížení.
Pravidlo Vyrovnávání zatížení je použité toodefine jak přenosy jsou distribuované toohello virtuálních počítačů. Můžete definovat hello front-endové konfiguraci protokolu IP pro příchozí provoz hello a hello back-end fondu tooreceive hello provozu IP, spolu s hello požadované zdrojový a cílový port. toomake se, že virtuální počítače pouze v pořádku přijímat přenosy, také definovat toouse test stavu hello.

Vytvořit pravidlo Vyrovnávání zatížení s [přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). Hello následující příklad vytvoří pravidlo Vyrovnávání zatížení s názvem *myLoadBalancerRule* a vyrovnává přenosy na portu *80*:

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Aktualizovat hello Vyrovnávání zatížení s [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a>Konfigurace virtuální sítě
Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte hello podpora prostředky virtuální sítě. Další informace o virtuálních sítích najdete v tématu hello [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.

### <a name="create-network-resources"></a>Vytvoření síťové prostředky
Vytvoření virtuální sítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Hello následující příklad vytvoří virtuální síť s názvem *myVnet* s *mySubnet*:

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create hello virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Vytvoření pravidla skupiny zabezpečení sítě s [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), pak vytvořte skupinu zabezpečení sítě s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Přidat hello toohello podsíť skupiny zabezpečení sítě s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) a aktualizujte hello virtuální síť s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). 

Hello následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem *myNetworkSecurityGroup* a použije je příliš*mySubnet*:

```powershell
# Create security rule config
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

# Create hello network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply hello network security group tooa subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update hello virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Virtuální síťové adaptéry jsou vytvořeny pomocí [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Hello následující příklad vytvoří tři virtuálních síťových karet. (Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro aplikace v rámci hello následující kroky). Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a přidat je nástroj pro vyrovnávání zatížení toohello:

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a>Vytváření virtuálních počítačů
tooimprove hello vysokou dostupnost vaší aplikace, umístit virtuální počítače v nastavení dostupnosti.

Vytvořit sadu s dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Hello následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

Nastavte uživatelské jméno a heslo správce pro virtuální počítače hello s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Nyní můžete vytvořit hello virtuálních počítačů s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Hello následující ukázka vytvoří tři virtuální počítače:

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

Trvá několik minut toocreate a nakonfigurovat všechny tři virtuální počítače.

### <a name="install-iis-with-custom-script-extension"></a>Nainstalovat službu ISS s rozšíření vlastních skriptů
V předchozích kurz [jak toocustomize virtuálního počítače s Windows](tutorial-automate-vm-deployment.md), jste se dozvěděli, jak tooautomate přizpůsobení virtuálního počítače s hello vlastní skript rozšíření pro Windows. Můžete použít hello stejné přístupu tooinstall a konfigurovat služby IIS na virtuální počítače.

Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello rozšíření vlastních skriptů. Hello rozšíření spustí `powershell Add-WindowsFeature Web-Server` tooinstall hello webový server služby IIS a poté aktualizace hello *Default.htm* stránky tooshow hello název hostitele hello virtuálních počítačů:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Nástroj pro vyrovnávání zatížení testu
Získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Hello následující příklad získá hello IP adresu pro *myPublicIP* vytvořili dříve:

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

Potom můžete zadat hello veřejnou IP adresu ve webovém prohlížeči tooa. Hello web se zobrazuje, včetně hello název hostitele virtuálního počítače hello tento nástroj pro vyrovnávání zatížení hello distribuované tooas provoz v hello následující ukázka:

![Spuštění webu IIS](./media/tutorial-load-balancer/running-iis-website.png)

Nástroj pro vyrovnávání zatížení toosee hello distribuuje provoz přes všechny tři virtuální počítače spuštěné aplikace, můžete můžete vynutit obnovení webového prohlížeče.


## <a name="add-and-remove-vms"></a>Přidání a odebrání virtuálních počítačů
Může být nutné tooperform údržby na hello virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému. toodeal zvýšení provozu tooyour aplikace, může být nutné tooadd dalších virtuálních počítačů. V této části se dozvíte, jak tooremove nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení hello.

### <a name="remove-a-vm-from-hello-load-balancer"></a>Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení hello
Získat hello síťová karta s [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), pak sada hello *pravidlo LoadBalancerBackendAddressPools* vlastnost hello virtuální síťovou kartu příliš*$null*. Nakonec aktualizujte virtuální síťový adaptér. hello:

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Nástroj pro vyrovnávání zatížení toosee hello distribuuje provoz přes hello zbývající dva virtuální počítače používající vaši aplikaci je můžete vynutit obnovení webového prohlížeče. Nyní můžete provést údržbu na hello virtuálních počítačů, jako je instalace aktualizací operačního systému nebo provádění restartování virtuálního počítače.

### <a name="add-a-vm-toohello-load-balancer"></a>Přidání toohello virtuálního počítače, služby pro vyrovnávání zatížení
Po provádění údržby virtuálních počítačů, nebo pokud potřebujete tooexpand kapacitu, nastavte hello *pravidlo LoadBalancerBackendAddressPools* vlastnost hello virtuální síťový adaptér toohello *BackendAddressPool* z [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

Získejte nástroj pro vyrovnávání zatížení hello:

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili pro vyrovnávání zatížení a připojené tooit virtuálních počítačů. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření pro vyrovnávání zatížení Azure
> * Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
> * Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení
> * Použít hello rozšíření vlastních skriptů toocreate základní webu IIS
> * Vytváření virtuálních počítačů a připojte tooa nástroj pro vyrovnávání zatížení
> * Zobrazit nástroj pro vyrovnávání zatížení v akci
> * Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení

Jak zálohy další kurz toolearn toohello toomanage sítě virtuálních počítačů.

> [!div class="nextstepaction"]
> [Správa virtuálních počítačů a virtuálních sítí](./tutorial-virtual-network.md)
