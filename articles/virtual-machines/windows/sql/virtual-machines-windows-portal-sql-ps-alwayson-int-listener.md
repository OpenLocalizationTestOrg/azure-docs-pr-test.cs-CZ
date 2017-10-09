---
title: "aaaConfigure vždy na dostupnosti naslouchací procesy skupiny – Microsoft Azure | Microsoft Docs"
description: "Naslouchací procesy skupiny dostupnosti nakonfigurujte na modelu hello Azure Resource Manager, pomocí interní nástroj pro jednu nebo více IP adres."
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/22/2017
ms.author: mikeray
ms.openlocfilehash: 81edfe2c2ea536d8dcec466f36fccf8bc0e02c2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Nakonfigurovat jeden nebo více vždy na dostupnosti naslouchací procesy skupiny - Resource Manager
Toto téma ukazuje, jak:

* Vytvořte interní nástroj pro skupiny dostupnosti systému SQL Server pomocí rutin prostředí PowerShell.
* Přidejte další IP adresy tooa Vyrovnávání zatížení pro víc než jedné skupiny dostupnosti. 

Naslouchací proces skupiny dostupnosti je název virtuální sítě, aby se klienti připojovali toofor přístup k databázi. Nástroj pro vyrovnávání zatížení na virtuálních počítačích Azure, obsahuje hello IP adresu pro naslouchací proces hello. směrování pro vyrovnávání zatížení Hello provoz toohello instance systému SQL Server, která naslouchá na portu testu hello. Skupiny dostupnosti se obvykle používá interní nástroj. K nástroji pro vyrovnávání zatížení interní Azure můžete hostovat jednu nebo více IP adres. Každou IP adresu používá port konkrétní test. Tento dokument ukazuje, jak toouse prostředí PowerShell toocreate nástroj pro vyrovnávání zatížení nebo přidejte IP adres tooan existující Vyrovnávání zatížení pro skupiny dostupnosti systému SQL Server. 

Hello možnost tooassign více IP adres tooan interní nástroj pro vyrovnávání zatížení je nové tooAzure a je dostupný jenom v modelu Resource Manager. toocomplete tuto úlohu je nutné toohave nasadit skupinu dostupnosti systému SQL Server na virtuálních počítačích Azure v modelu Resource Manager. Virtuální počítače systému SQL Server musí patřit toohello stejné skupině dostupnosti. Můžete použít hello [šablony aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically vytvoření skupiny dostupnosti hello ve službě Správce prostředků Azure. Tato šablona automaticky vytvoří hello skupiny dostupnosti, včetně služby Vyrovnávání zatížení interní hello za vás. Pokud dáváte přednost, můžete [ruční konfigurace skupiny dostupnosti Always On](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Toto téma vyžaduje vaše skupiny dostupnosti jsou již nakonfigurována.  

Související témata:

* [Konfigurace skupin dostupnosti AlwaysOn na virtuálním počítači Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Nakonfigurujte připojení VNet-to-VNet s použitím Azure Resource Manageru a prostředí PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-hello-windows-firewall"></a>Konfigurace hello brány Windows Firewall
Nakonfigurujte hello brány Windows Firewall tooallow systému SQL Server přístup. pravidla brány firewall Hello umožňují používat porty toohello připojení TCP tak, že instance systému SQL Server hello a testu hello naslouchací proces. Podrobné pokyny najdete v tématu [konfigurace brány Windows Firewall pro přístup k databázovému stroji](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Vytvoření příchozího pravidla pro hello port serveru SQL Server a port testu hello.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Ukázkový skript: Vytvoření Vyrovnávání zatížení interní pomocí prostředí PowerShell
> [!NOTE]
> Pokud jste vytvořili vaší skupiny dostupnosti s hello [šablony aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), již bylo vytvořeno hello interní vyrovnávání zátěže. 
> 
> 

Hello následující skript prostředí PowerShell vytvoří interní nástroj, nakonfiguruje pravidla Vyrovnávání zatížení hello a nastaví IP adresu pro nástroj pro vyrovnávání zatížení hello. skript hello toorun, otevřete Windows PowerShell ISE a vložte hello skriptu v podokně skriptu hello. Použití `Login-AzureRMAccount` toolog v tooPowerShell. Pokud máte víc předplatných Azure, použijte `Select-AzureRmSubscription ` tooset hello předplatného. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # hello Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # hello Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for hello front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for hello back-end configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="Add-IP"></a>Ukázkový skript: přidejte k IP adresu tooan existující pro vyrovnávání zatížení v prostředí PowerShell
toouse více než jeden dostupnosti skupiny, přidejte k další IP adresu toohello pro vyrovnávání zatížení. Každou IP adresu vyžaduje vlastní pravidla Vyrovnávání zatížení, port testu a front port.

Hello front-end port je port hello, aplikace použít instanci systému SQL Server toohello tooconnect. IP adresy pro různé dostupnosti skupiny můžete použít hello stejný port front-endu.

> [!NOTE]
> Pro skupiny dostupnosti systému SQL Server vyžaduje každou IP adresu, port konkrétní testu. Například pokud jednu IP adresu zařízení na Vyrovnávání zatížení používá port testu 59999, žádné jiné IP adresy na tento nástroj pro vyrovnávání zatížení můžete použít port testu 59999.

* Informace o omezeních pro vyrovnávání zatížení najdete v tématu **privátní front-endu IP adresu na nástroj pro vyrovnávání zatížení** pod [omezení sítě - Správce Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).
* Informace o omezeních skupiny dostupnosti naleznete v tématu [omezení (skupiny dostupnosti)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Hello následující skript přidá novou IP adresu tooan existující Vyrovnávání zatížení. Hello ILB používá port naslouchacího procesu hello hello zátěže front-end port. Hello port, který SQL Server naslouchá na může být tento port. Pro výchozí instance systému SQL Server hello port je 1433. pravidlo pro skupinu dostupnosti Vyrovnávání zatížení Hello vyžaduje plovoucí IP (přímá odpověď ze serveru) tak, aby hello back-end port stejné hello jako hello front-end port. Aktualizujte hello proměnné prostředí. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```

## <a name="configure-hello-listener"></a>Konfigurace naslouchací proces hello

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-hello-listener-port-in-sql-server-management-studio"></a>Nastavit port naslouchacího procesu hello v aplikaci SQL Server Management Studio

1. Spusťte SQL Server Management Studio a připojte toohello primární repliky.

1. Přejděte příliš**vysoké dostupnosti AlwaysOn** | **skupiny dostupnosti** | **naslouchací procesy skupiny dostupnosti**. 

1. Teď byste měli vidět název hello naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání. Klikněte pravým tlačítkem na název naslouchacího procesu hello a klikněte na tlačítko **vlastnosti**.

1. V hello **Port** zadejte číslo portu naslouchacího procesu skupiny dostupnosti hello hello pomocí hello $EndpointPort jste použili předtím (1433 byla výchozí hello), pak klikněte na tlačítko **OK**.

## <a name="test-hello-connection-toohello-listener"></a>Naslouchací proces toohello testovací hello připojení

tootest hello připojení:

1. RDP tooa systému SQL Server, který je v hello stejné virtuální síti však není vlastní hello repliky. To může být hello jiný SQL Server v clusteru hello.

1. Použití **sqlcmd** nástroj tootest hello připojení. Například vytvoří hello následující skript **sqlcmd** připojení toohello primární repliky prostřednictvím hello naslouchací proces s ověřováním systému Windows:
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    Pokud naslouchací proces hello používá port jiný než hello výchozí port (1433), zadejte hello port v hello připojovací řetězec. Například hello následujícího příkazu sqlcmd připojuje tooa naslouchání na portu 1435: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Hello SQLCMD připojení automaticky připojí toowhichever instanci systému SQL Server hostitele hello primární repliky. 

> [!NOTE]
> Ujistěte se, že je otevřen v bráně firewall hello obou serverů SQL hello port, který zadáte. Oba servery vyžadují příchozí pravidlo pro hello port TCP, který používáte. V tématu [přidat nebo upravit pravidlo brány Firewall](http://technet.microsoft.com/library/cc753558.aspx) Další informace. 
> 
> 

## <a name="guidelines-and-limitations"></a>Pokyny a omezení
Vezměte na vědomí následující pokyny na naslouchací proces skupiny dostupnosti v Azure pomocí nástroje pro vyrovnávání zatížení pro vnitřní hello:

* S nástrojem pro vyrovnávání zatížení pro vnitřní, můžete přístup pouze k naslouchání hello z v rámci hello stejné virtuální síti.


## <a name="for-more-information"></a>Další informace
Další informace najdete v tématu [skupiny dostupnosti nakonfigurujte Always On ve virtuálním počítači Azure ručně](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

## <a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell
Použijte hello následující toocreate rutiny prostředí PowerShell interní nástroj pro virtuální počítače Azure.

* [Nové AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) vytvoří nástroj pro vyrovnávání zatížení. 
* [Nové AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) vytvoří konfiguraci front-end IP adresy pro vyrovnávání zatížení. 
* [Nové AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) vytvoří pravidlo konfigurace pro vyrovnávání zatížení. 
* [Nové AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) vytvoří konfiguraci fondu adres back-end pro vyrovnávání zatížení. 
* [Nové AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) vytvoří test konfigurace pro vyrovnávání zatížení.
* [Odebrat AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) Odebere skupinu prostředků Azure pro vyrovnávání zatížení.
