---
title: "aaaSimulated hybridní cloud testovací prostředí | Microsoft Docs"
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
ms.openlocfilehash: 6063022e373c2b3564e040c4fbee122e2438974b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Nastavení simulovaného hybridního cloudového prostředí pro testování
Tento článek vás provede jednotlivými kroky vytváření simulované hybridní prostředí cloudu s Microsoft Azure pomocí dvou virtuálních sítí Azure. Zde je hello Výsledná konfigurace.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

To simuluje provozním prostředí hybridní cloud a skládá se z:

* Simulované a zjednodušenou místní síť hostované ve virtuální sítě Azure (hello testovací laboratoř virtuální sítě).
* Simulované mezi různými místy virtuální síť hostované v Azure (TestVNET).
* Připojení VNet-to-VNet mezi hello dvě virtuální sítě.
* Sekundární řadič domény ve virtuální síti TestVNET hello.

To poskytuje že základ a běžné od bodu, ze kterých lze:

* Vývoj a testování aplikací v simulovaném hybridní cloudové prostředí.
* Vytváření konfigurací testů počítačů, některé v rámci hello testovací laboratoř virtuální sítě a některé v rámci hello TestVNET virtuální sítě, toosimulate hybridní cloudové IT úlohy.

Existují čtyři hlavní fáze toosetting toto hybridní cloud testovací prostředí:

1. Nakonfigurujte hello testovací laboratoř virtuální sítě.
2. Vytvoření virtuální sítě mezi různými místy hello.
3. Vytvořte připojení VPN hello VNet-to-VNet.
4. Konfigurace počítače DC2. 

Tato konfigurace vyžaduje předplatné Azure. Pokud máte předplatné MSDN nebo v sadě Visual Studio, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

> [!NOTE]
> Virtuální počítače a brány virtuální sítě v Azure zpoplatněná nákladů na probíhající peněžní při spuštění. Tyto poplatky je účtován na vašem webu MSDN nebo placené předplatné. Služby Azure VPN gateway je implementovaná jako sada dva virtuální počítače Azure. toominimize hello náklady, vytvořit hello testovacího prostředí a proveďte potřebné testováním a ukázkový co nejrychleji.
> 
> 

## <a name="phase-1-configure-hello-testlab-virtual-network"></a>Fáze 1: Konfigurace hello testovací laboratoř virtuální sítě
Použijte pokyny hello v hello [základní konfigurace testovacího prostředí](https://technet.microsoft.com/library/mt771177.aspx) tématu tooconfigure hello DC1, APP1 a CLIENT1 počítačů v hello virtuální síť Azure s názvem testovací laboratoř. 

V dalším kroku spusťte příkazovém řádku prostředí Azure PowerShell.

> [!NOTE]
> Hello následující sady příkazů pomocí Azure PowerShell 1.0 nebo novější.
> 
> 

Přihlaste se tooyour účtu.

    Login-AzureRMAccount

Získáte název odběru pomocí hello následující příkaz.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Nastavte vašeho předplatného Azure. Použití hello stejné předplatné, kterou jste použili toobuild základní konfigurace v fáze 1. Vše v rámci hello uvozovky, včetně hello < a > znaky, nahraďte hello správný název.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Dál přidejte brány podsítě toohello testovací laboratoř virtuální síť základní konfigurace, které budou použité toohost hello služba Azure gateway.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

V dalším kroku požádat o veřejné IP adresy tooassign toohello bránu pro virtuální síť testovací laboratoř hello.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Dál vytvořte bránu.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Uvědomte si, že nové brány může trvat 20 minut nebo další toocreate.

Z hello k portálu Azure do místního počítače, připojení tooDC1 hello přihlašovací údaje pro corp\uživatel1. domény CORP hello tooconfigure tak, aby počítače a uživatelé používat své místní řadič domény pro ověřování, spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell na úrovni správce na počítači DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-hello-testvnet-virtual-network"></a>Fáze 2: Vytvoření virtuální sítě TestVNET hello
Nejprve vytvořte hello TestVNET virtuální sítě a chránit pomocí skupiny zabezpečení sítě.

    $rgName="<name of hello resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP tooall VMs on hello subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Žádost o veřejný toobe adres IP v dalším kroku přidělené toohello brány virtuální sítě hello TestVNET a vytvořte bránu.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-hello-vnet-to-vnet-connection"></a>Fáze 3: Vytvoření připojení hello VNet-to-VNet
Nejdřív získejte náhodné, kryptograficky silnou, 32 znaků předsdílený klíč ze správce sítě nebo zabezpečení. Můžete také použít informace hello [vytvořit náhodný řetězec pro předsdílený klíč pomocí protokolu IPsec](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain předsdílený klíč.

Pak pomocí těchto příkazů toocreate hello VPN VNet-to-VNet připojení, které můžete provést některé toocomplete čas.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Po několika minutách by se mělo vytvořit hello připojení.

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a>Fáze 4: Konfigurace počítače DC2
Nejprve vytvořte virtuálního počítače pro řadič domény DC2. Spuštění těchto příkazů na příkazovém řádku prostředí Azure PowerShell hello v místním počítači.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<hello storage account name for hello base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

V dalším kroku připojte toohello nový řadič domény DC2 virtuální počítač z hello portálu Azure.

V dalším kroku nakonfigurujte přenosem tooallow pravidla brány Windows Firewall pro testování základní připojení. Z prostředí Windows PowerShell na úrovni správce příkazového řádku na počítači DC2 spuštění těchto příkazů.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

příkaz ping Hello by měl mít za následek čtyři úspěšné odpovědi z IP adresy 10.0.0.4. Toto je test přenosů mezi hello připojení VNet-to-VNet.

Dál přidejte hello navíc datový disk na počítači DC2 jako nový svazek s písmenem jednotky hello F:.

1. V levém podokně hello správce serveru, klikněte na **Souborová služba a služba úložiště**a potom klikněte na **disky**.
2. V podokně obsahu hello v hello **disky** klikněte na možnost **disku 2** (s hello **oddílu** nastavit příliš**neznámé**).
3. Klikněte na tlačítko **úlohy**a potom klikněte na **nový svazek**.
4. Na hello před zahájením stránku hello Průvodce vytvořením svazku, klikněte na tlačítko **Další**.
5. Na serveru vyberte hello hello a stránka disku, klikněte na tlačítko **disku 2**a potom klikněte na **Další**. Po zobrazení výzvy klikněte na tlačítko **OK**.
6. Na hello zadat velikost hello hello svazku stránky, klikněte na tlačítko **Další**.
7. Na hello přiřazovat tooa jednotky písmeno nebo složka stránky, klikněte na tlačítko **Další**.
8. Na stránce nastavení systému hello vybrat soubor, klikněte na tlačítko **Další**.
9. Na stránce Potvrdit vybrané možnosti hello klikněte **vytvořit**.
10. Po dokončení klikněte na tlačítko **Zavřít**.

V dalším kroku nakonfigurujte DC2 jako repliku řadiče domény pro doménu hello corp.contoso.com. Spusťte tyto příkazy z příkazového řádku prostředí Windows PowerShell hello na počítači DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Všimněte si, že jste výzvami toosupply obě hello CORP\User1 heslo a heslo Directory režimu obnovení služeb (DSRM) a toorestart DC2.

Teď, když hello TestVNET virtuální sítě má svou vlastní server DNS (DC2), musíte nakonfigurovat hello TestVNET virtuální sítě toouse tento server DNS.

1. V levém podokně hello Dobrý den portálu Azure klikněte na ikonu hello virtuální sítě a pak klikněte na tlačítko **TestVNET**.
2. Na hello **nastavení** , klikněte na **servery DNS**.
3. V **primárního serveru DNS**, typ **192.168.0.4** tooreplace 10.0.0.4.
4. Klikněte na **Uložit**.

Toto je aktuální konfiguraci. 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Simulované hybridní cloudové prostředí je nyní připraven pro testování.

## <a name="next-step"></a>Další krok
* Nastavení [webové, řádek obchodní aplikace](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) v tomto prostředí.

