---
title: "Simulované hybridní cloud testovací prostředí | Microsoft Docs"
description: "Vytvoření simulovaného hybridní cloudové prostředí pro IT pro nebo vývoj testování, pomocí dvou virtuálních sítí Azure a připojení VNet-to-VNet."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ca5bf294-8172-44a9-8fed-d6f38d345364
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: 4ec6f079b762a25894d822bfc098ea5442a1f7e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Nastavení simulovaného hybridního cloudového prostředí pro testování
Tento článek vás provede jednotlivými kroky vytváření simulované hybridní prostředí cloudu s Microsoft Azure pomocí dvou virtuálních sítí Azure. Zde je Výsledná konfigurace.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

To simuluje provozním prostředí hybridní cloud a skládá se z:

* Simulované a zjednodušenou místní síť hostované v Azure virtuální sítě (virtuální sítě testovací laboratoř).
* Simulované mezi různými místy virtuální síť hostované v Azure (TestVNET).
* Připojení VNet-to-VNet mezi dvěma virtuálními sítěmi.
* Sekundární řadič domény ve virtuální síti TestVNET.

To poskytuje že základ a běžné od bodu, ze kterých lze:

* Vývoj a testování aplikací v simulovaném hybridní cloudové prostředí.
* Vytváření konfigurací testů počítačů, některé v rámci virtuální sítě testovací laboratoř a některé v rámci TestVNET virtuální sítě, aby simuloval hybridní cloudové IT úlohy.

Existují čtyři hlavní fáze k nastavení tohoto testovacího prostředí hybridního cloudu:

1. Nakonfigurujte virtuální síť testovací laboratoř.
2. Vytvoření virtuální sítě mezi různými místy.
3. Vytvoření připojení k síti VPN VNet-to-VNet.
4. Konfigurace počítače DC2. 

Tato konfigurace vyžaduje předplatné Azure. Pokud máte předplatné MSDN nebo v sadě Visual Studio, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

> [!NOTE]
> Virtuální počítače a brány virtuální sítě v Azure zpoplatněná nákladů na probíhající peněžní při spuštění. Tyto poplatky je účtován na vašem webu MSDN nebo placené předplatné. Služby Azure VPN gateway je implementovaná jako sada dva virtuální počítače Azure. Chcete-li minimalizovat náklady, vytvořit testovací prostředí a provádět potřebné testováním a ukázkový co nejrychleji.
> 
> 

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Fáze 1: Konfigurace virtuální sítě testovací laboratoř
Postupujte podle pokynů v [základní konfigurace testovacího prostředí](https://technet.microsoft.com/library/mt771177.aspx) tématu ke konfiguraci počítačů DC1, APP1 a CLIENT1 ve virtuální síti Azure, s názvem testovací laboratoř. 

V dalším kroku spusťte příkazovém řádku prostředí Azure PowerShell.

> [!NOTE]
> Tento příkaz nastaví použití Azure PowerShell 1.0 nebo novější.
> 
> 

Přihlaste se ke svému účtu.

    Login-AzureRMAccount

Získáte název odběru, pomocí následujícího příkazu.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Nastavte vašeho předplatného Azure. Použijte stejné předplatné, které jste použili k vytvoření základní konfigurace fáze 1. Nahraďte všechna data v uvozovkách, včetně < a > znaků, se správným názvem.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Dále přidáte podsíť brány virtuální sítě testovací laboratoř základní konfigurace, který bude použit k hostování bránu Azure.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

V dalším kroku žádost veřejnou IP adresu přiřadit k bráně virtuální sítě testovací laboratoř.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Dál vytvořte bránu.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Mějte na paměti, že nové brány může trvat 20 minut nebo déle vytvořit.

Z portálu Azure k místnímu počítači připojte k řadiči domény DC1 se přihlašovací údaje pro corp\uživatel1. Konfigurovat domény CORP tak, aby počítače a uživatelé používat své místní řadič domény pro ověřování, spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell na úrovni správce na počítači DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-the-testvnet-virtual-network"></a>Fáze 2: Vytvoření virtuální sítě TestVNET
Nejdřív vytvoříte virtuální síť TestVNET a chránit pomocí skupiny zabezpečení sítě.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

V dalším kroku žádost veřejnou adresu IP, která bude přidělena pro bránu virtuální sítě TestVNET a vytvořte bránu.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-the-vnet-to-vnet-connection"></a>Fáze 3: Vytvoření připojení VNet-to-VNet
Nejdřív získejte náhodné, kryptograficky silnou, 32 znaků předsdílený klíč ze správce sítě nebo zabezpečení. Alternativně použijte informace v [vytvořit náhodný řetězec pro předsdílený klíč pomocí protokolu IPsec](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) získat předsdílený klíč.

Dále použijte tyto příkazy k vytvoření připojení VPN VNet-to-VNet, což může trvat delší dobu.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Po několika minutách by se mělo vytvořit připojení.

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a>Fáze 4: Konfigurace počítače DC2
Nejprve vytvořte virtuálního počítače pro řadič domény DC2. Spuštění těchto příkazů na příkazovém řádku prostředí Azure PowerShell v místním počítači.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

V dalším kroku připojte k nového virtuálního počítače DC2 z portálu Azure.

V dalším kroku nakonfigurujte brány Windows Firewall pravidlo umožňující přenos pro testování základní připojení. Z prostředí Windows PowerShell na úrovni správce příkazového řádku na počítači DC2 spuštění těchto příkazů.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Příkaz ping by měl mít za následek čtyři úspěšné odpovědi z IP adresy 10.0.0.4. Toto je test provozu prostřednictvím připojení VNet-to-VNet.

V dalším kroku přidejte disk doplňující data na počítači DC2 jako nový svazek s písmenem jednotky F:.

1. V levém podokně ve Správci serveru klikněte na tlačítko **Souborová služba a služba úložiště**a potom klikněte na **disky**.
2. V podokně obsahu v **disky** klikněte na možnost **disku 2** (s **oddílu** nastavena na **neznámé**).
3. Klikněte na tlačítko **úlohy**a potom klikněte na **nový svazek**.
4. Na stránce než můžete začít stránky Průvodce vytvořením svazku, klikněte na tlačítko **Další**.
5. Na stránce vyberte server a disk stránky, klikněte na tlačítko **disku 2**a potom klikněte na **Další**. Po zobrazení výzvy klikněte na tlačítko **OK**.
6. Na zadání velikosti svazku stránky, klikněte na tlačítko **Další**.
7. Na přiřazení disku písmeno nebo složka stránku, klikněte na tlačítko **Další**.
8. Na stránce nastavení systému vybrat soubor, klikněte na tlačítko **Další**.
9. Na stránce Potvrdit vybrané možnosti, klikněte na tlačítko **vytvořit**.
10. Po dokončení klikněte na tlačítko **Zavřít**.

V dalším kroku nakonfigurujte DC2 jako repliku řadiče domény v doméně corp.contoso.com. Spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell na počítači DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Všimněte si, že budete vyzváni k zadání hesla corp\uživatel1 a heslo Directory režimu obnovení služeb (DSRM) a k restartování počítače DC2.

Nyní, když virtuální sítě TestVNET vlastní server DNS (DC2), je nutné nakonfigurovat virtuální síť TestVNET používat tento server DNS.

1. V levém podokně portálu Azure, klikněte na ikonu virtuální sítě a pak klikněte na tlačítko **TestVNET**.
2. Na **nastavení** , klikněte na **servery DNS**.
3. V **primárního serveru DNS**, typ **192.168.0.4** nahradit 10.0.0.4.
4. Klikněte na **Uložit**.

Toto je aktuální konfiguraci. 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Simulované hybridní cloudové prostředí je nyní připraven pro testování.

## <a name="next-step"></a>Další krok
* Nastavení [webové, řádek obchodní aplikace](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) v tomto prostředí.

