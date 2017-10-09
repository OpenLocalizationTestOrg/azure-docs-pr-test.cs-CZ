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
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a><span data-ttu-id="95a22-103">Nakonfigurovat jeden nebo více vždy na dostupnosti naslouchací procesy skupiny - Resource Manager</span><span class="sxs-lookup"><span data-stu-id="95a22-103">Configure one or more Always On availability group listeners - Resource Manager</span></span>
<span data-ttu-id="95a22-104">Toto téma ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="95a22-104">This topic shows how to:</span></span>

* <span data-ttu-id="95a22-105">Vytvořte interní nástroj pro skupiny dostupnosti systému SQL Server pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="95a22-105">Create an internal load balancer for SQL Server availability groups using PowerShell cmdlets.</span></span>
* <span data-ttu-id="95a22-106">Přidejte další IP adresy tooa Vyrovnávání zatížení pro víc než jedné skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="95a22-106">Add additional IP addresses tooa load balancer for more than one availability group.</span></span> 

<span data-ttu-id="95a22-107">Naslouchací proces skupiny dostupnosti je název virtuální sítě, aby se klienti připojovali toofor přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="95a22-107">An availability group listener is a virtual network name that clients connect toofor database access.</span></span> <span data-ttu-id="95a22-108">Nástroj pro vyrovnávání zatížení na virtuálních počítačích Azure, obsahuje hello IP adresu pro naslouchací proces hello.</span><span class="sxs-lookup"><span data-stu-id="95a22-108">On Azure virtual machines, a load balancer holds hello IP address for hello listener.</span></span> <span data-ttu-id="95a22-109">směrování pro vyrovnávání zatížení Hello provoz toohello instance systému SQL Server, která naslouchá na portu testu hello.</span><span class="sxs-lookup"><span data-stu-id="95a22-109">hello load balancer routes traffic toohello instance of SQL Server that is listening on hello probe port.</span></span> <span data-ttu-id="95a22-110">Skupiny dostupnosti se obvykle používá interní nástroj.</span><span class="sxs-lookup"><span data-stu-id="95a22-110">Usually, an availability group uses an internal load balancer.</span></span> <span data-ttu-id="95a22-111">K nástroji pro vyrovnávání zatížení interní Azure můžete hostovat jednu nebo více IP adres.</span><span class="sxs-lookup"><span data-stu-id="95a22-111">An Azure internal load balancer can host one or many IP addresses.</span></span> <span data-ttu-id="95a22-112">Každou IP adresu používá port konkrétní test.</span><span class="sxs-lookup"><span data-stu-id="95a22-112">Each IP address uses a specific probe port.</span></span> <span data-ttu-id="95a22-113">Tento dokument ukazuje, jak toouse prostředí PowerShell toocreate nástroj pro vyrovnávání zatížení nebo přidejte IP adres tooan existující Vyrovnávání zatížení pro skupiny dostupnosti systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="95a22-113">This document shows how toouse PowerShell toocreate a load balancer, or add IP addresses tooan existing load balancer for SQL Server availability groups.</span></span> 

<span data-ttu-id="95a22-114">Hello možnost tooassign více IP adres tooan interní nástroj pro vyrovnávání zatížení je nové tooAzure a je dostupný jenom v modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="95a22-114">hello ability tooassign multiple IP addresses tooan internal load balancer is new tooAzure and is only available in Resource Manager model.</span></span> <span data-ttu-id="95a22-115">toocomplete tuto úlohu je nutné toohave nasadit skupinu dostupnosti systému SQL Server na virtuálních počítačích Azure v modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="95a22-115">toocomplete this task, you need toohave a SQL Server availability group deployed on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="95a22-116">Virtuální počítače systému SQL Server musí patřit toohello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="95a22-116">Both SQL Server virtual machines must belong toohello same availability set.</span></span> <span data-ttu-id="95a22-117">Můžete použít hello [šablony aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically vytvoření skupiny dostupnosti hello ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="95a22-117">You can use hello [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) tooautomatically create hello availability group in Azure Resource Manager.</span></span> <span data-ttu-id="95a22-118">Tato šablona automaticky vytvoří hello skupiny dostupnosti, včetně služby Vyrovnávání zatížení interní hello za vás.</span><span class="sxs-lookup"><span data-stu-id="95a22-118">This template automatically creates hello availability group, including hello internal load balancer for you.</span></span> <span data-ttu-id="95a22-119">Pokud dáváte přednost, můžete [ruční konfigurace skupiny dostupnosti Always On](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="95a22-119">If you prefer, you can [manually configure an Always On availability group](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

<span data-ttu-id="95a22-120">Toto téma vyžaduje vaše skupiny dostupnosti jsou již nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="95a22-120">This topic requires that your availability groups are already configured.</span></span>  

<span data-ttu-id="95a22-121">Související témata:</span><span class="sxs-lookup"><span data-stu-id="95a22-121">Related topics include:</span></span>

* [<span data-ttu-id="95a22-122">Konfigurace skupin dostupnosti AlwaysOn na virtuálním počítači Azure (GUI)</span><span class="sxs-lookup"><span data-stu-id="95a22-122">Configure AlwaysOn Availability Groups in Azure VM (GUI)</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [<span data-ttu-id="95a22-123">Nakonfigurujte připojení VNet-to-VNet s použitím Azure Resource Manageru a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="95a22-123">Configure a VNet-to-VNet connection by using Azure Resource Manager and PowerShell</span></span>](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-hello-windows-firewall"></a><span data-ttu-id="95a22-124">Konfigurace hello brány Windows Firewall</span><span class="sxs-lookup"><span data-stu-id="95a22-124">Configure hello Windows Firewall</span></span>
<span data-ttu-id="95a22-125">Nakonfigurujte hello brány Windows Firewall tooallow systému SQL Server přístup.</span><span class="sxs-lookup"><span data-stu-id="95a22-125">Configure hello Windows Firewall tooallow SQL Server access.</span></span> <span data-ttu-id="95a22-126">pravidla brány firewall Hello umožňují používat porty toohello připojení TCP tak, že instance systému SQL Server hello a testu hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="95a22-126">hello firewall rules allow TCP connections toohello ports use by hello SQL Server instance, and hello listener probe.</span></span> <span data-ttu-id="95a22-127">Podrobné pokyny najdete v tématu [konfigurace brány Windows Firewall pro přístup k databázovému stroji](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="95a22-127">For detailed instructions, see [Configure a Windows Firewall for Database Engine Access](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span></span> <span data-ttu-id="95a22-128">Vytvoření příchozího pravidla pro hello port serveru SQL Server a port testu hello.</span><span class="sxs-lookup"><span data-stu-id="95a22-128">Create an inbound rule for hello SQL Server port and for hello probe port.</span></span>

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a><span data-ttu-id="95a22-129">Ukázkový skript: Vytvoření Vyrovnávání zatížení interní pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="95a22-129">Example Script: Create an internal load balancer with PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="95a22-130">Pokud jste vytvořili vaší skupiny dostupnosti s hello [šablony aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), již bylo vytvořeno hello interní vyrovnávání zátěže.</span><span class="sxs-lookup"><span data-stu-id="95a22-130">If you created your availability group with hello [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), hello internal load balancer was already created.</span></span> 
> 
> 

<span data-ttu-id="95a22-131">Hello následující skript prostředí PowerShell vytvoří interní nástroj, nakonfiguruje pravidla Vyrovnávání zatížení hello a nastaví IP adresu pro nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="95a22-131">hello following PowerShell script creates an internal load balancer, configures hello load balancing rules, and sets an IP address for hello load balancer.</span></span> <span data-ttu-id="95a22-132">skript hello toorun, otevřete Windows PowerShell ISE a vložte hello skriptu v podokně skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="95a22-132">toorun hello script, open Windows PowerShell ISE, and paste hello script in hello Script pane.</span></span> <span data-ttu-id="95a22-133">Použití `Login-AzureRMAccount` toolog v tooPowerShell.</span><span class="sxs-lookup"><span data-stu-id="95a22-133">Use `Login-AzureRMAccount` toolog in tooPowerShell.</span></span> <span data-ttu-id="95a22-134">Pokud máte víc předplatných Azure, použijte `Select-AzureRmSubscription ` tooset hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="95a22-134">If you have multiple Azure subscriptions, use `Select-AzureRmSubscription ` tooset hello subscription.</span></span> 

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

## <span data-ttu-id="95a22-135"><a name="Add-IP"></a>Ukázkový skript: přidejte k IP adresu tooan existující pro vyrovnávání zatížení v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="95a22-135"><a name="Add-IP"></a> Example script: Add an IP address tooan existing load balancer with PowerShell</span></span>
<span data-ttu-id="95a22-136">toouse více než jeden dostupnosti skupiny, přidejte k další IP adresu toohello pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="95a22-136">toouse more than one availability group, add an additional IP address toohello load balancer.</span></span> <span data-ttu-id="95a22-137">Každou IP adresu vyžaduje vlastní pravidla Vyrovnávání zatížení, port testu a front port.</span><span class="sxs-lookup"><span data-stu-id="95a22-137">Each IP address requires its own load balancing rule, probe port, and front port.</span></span>

<span data-ttu-id="95a22-138">Hello front-end port je port hello, aplikace použít instanci systému SQL Server toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="95a22-138">hello front-end port is hello port that applications use tooconnect toohello SQL Server instance.</span></span> <span data-ttu-id="95a22-139">IP adresy pro různé dostupnosti skupiny můžete použít hello stejný port front-endu.</span><span class="sxs-lookup"><span data-stu-id="95a22-139">IP addresses for different availability groups can use hello same front-end port.</span></span>

> [!NOTE]
> <span data-ttu-id="95a22-140">Pro skupiny dostupnosti systému SQL Server vyžaduje každou IP adresu, port konkrétní testu.</span><span class="sxs-lookup"><span data-stu-id="95a22-140">For SQL Server availability groups, each IP address requires a specific probe port.</span></span> <span data-ttu-id="95a22-141">Například pokud jednu IP adresu zařízení na Vyrovnávání zatížení používá port testu 59999, žádné jiné IP adresy na tento nástroj pro vyrovnávání zatížení můžete použít port testu 59999.</span><span class="sxs-lookup"><span data-stu-id="95a22-141">For example, if one IP address on a load balancer uses probe port 59999, no other IP addresses on that load balancer can use probe port 59999.</span></span>

* <span data-ttu-id="95a22-142">Informace o omezeních pro vyrovnávání zatížení najdete v tématu **privátní front-endu IP adresu na nástroj pro vyrovnávání zatížení** pod [omezení sítě - Správce Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span><span class="sxs-lookup"><span data-stu-id="95a22-142">For information about load balancer limits, see **Private front end IP per load balancer** under [Networking Limits - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span></span>
* <span data-ttu-id="95a22-143">Informace o omezeních skupiny dostupnosti naleznete v tématu [omezení (skupiny dostupnosti)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span><span class="sxs-lookup"><span data-stu-id="95a22-143">For information about availability group limits, see [Restrictions (Availability Groups)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span></span>

<span data-ttu-id="95a22-144">Hello následující skript přidá novou IP adresu tooan existující Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="95a22-144">hello following script adds a new IP address tooan existing load balancer.</span></span> <span data-ttu-id="95a22-145">Hello ILB používá port naslouchacího procesu hello hello zátěže front-end port.</span><span class="sxs-lookup"><span data-stu-id="95a22-145">hello ILB uses hello listener port for hello load balancing front-end port.</span></span> <span data-ttu-id="95a22-146">Hello port, který SQL Server naslouchá na může být tento port.</span><span class="sxs-lookup"><span data-stu-id="95a22-146">This port can be hello port that SQL Server is listening on.</span></span> <span data-ttu-id="95a22-147">Pro výchozí instance systému SQL Server hello port je 1433.</span><span class="sxs-lookup"><span data-stu-id="95a22-147">For default instances of SQL Server, hello port is 1433.</span></span> <span data-ttu-id="95a22-148">pravidlo pro skupinu dostupnosti Vyrovnávání zatížení Hello vyžaduje plovoucí IP (přímá odpověď ze serveru) tak, aby hello back-end port stejné hello jako hello front-end port.</span><span class="sxs-lookup"><span data-stu-id="95a22-148">hello load balancing rule for an availability group requires a floating IP (direct server return) so hello back-end port is hello same as hello front-end port.</span></span> <span data-ttu-id="95a22-149">Aktualizujte hello proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="95a22-149">Update hello variables for your environment.</span></span> 

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

## <a name="configure-hello-listener"></a><span data-ttu-id="95a22-150">Konfigurace naslouchací proces hello</span><span class="sxs-lookup"><span data-stu-id="95a22-150">Configure hello listener</span></span>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-hello-listener-port-in-sql-server-management-studio"></a><span data-ttu-id="95a22-151">Nastavit port naslouchacího procesu hello v aplikaci SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="95a22-151">Set hello listener port in SQL Server Management Studio</span></span>

1. <span data-ttu-id="95a22-152">Spusťte SQL Server Management Studio a připojte toohello primární repliky.</span><span class="sxs-lookup"><span data-stu-id="95a22-152">Launch SQL Server Management Studio and connect toohello primary replica.</span></span>

1. <span data-ttu-id="95a22-153">Přejděte příliš**vysoké dostupnosti AlwaysOn** | **skupiny dostupnosti** | **naslouchací procesy skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="95a22-153">Navigate too**AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span> 

1. <span data-ttu-id="95a22-154">Teď byste měli vidět název hello naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="95a22-154">You should now see hello listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="95a22-155">Klikněte pravým tlačítkem na název naslouchacího procesu hello a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="95a22-155">Right-click hello listener name and click **Properties**.</span></span>

1. <span data-ttu-id="95a22-156">V hello **Port** zadejte číslo portu naslouchacího procesu skupiny dostupnosti hello hello pomocí hello $EndpointPort jste použili předtím (1433 byla výchozí hello), pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="95a22-156">In hello **Port** box, specify hello port number for hello availability group listener by using hello $EndpointPort you used earlier (1433 was hello default), then click **OK**.</span></span>

## <a name="test-hello-connection-toohello-listener"></a><span data-ttu-id="95a22-157">Naslouchací proces toohello testovací hello připojení</span><span class="sxs-lookup"><span data-stu-id="95a22-157">Test hello connection toohello listener</span></span>

<span data-ttu-id="95a22-158">tootest hello připojení:</span><span class="sxs-lookup"><span data-stu-id="95a22-158">tootest hello connection:</span></span>

1. <span data-ttu-id="95a22-159">RDP tooa systému SQL Server, který je v hello stejné virtuální síti však není vlastní hello repliky.</span><span class="sxs-lookup"><span data-stu-id="95a22-159">RDP tooa SQL Server that is in hello same virtual network, but does not own hello replica.</span></span> <span data-ttu-id="95a22-160">To může být hello jiný SQL Server v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="95a22-160">This can be hello other SQL Server in hello cluster.</span></span>

1. <span data-ttu-id="95a22-161">Použití **sqlcmd** nástroj tootest hello připojení.</span><span class="sxs-lookup"><span data-stu-id="95a22-161">Use **sqlcmd** utility tootest hello connection.</span></span> <span data-ttu-id="95a22-162">Například vytvoří hello následující skript **sqlcmd** připojení toohello primární repliky prostřednictvím hello naslouchací proces s ověřováním systému Windows:</span><span class="sxs-lookup"><span data-stu-id="95a22-162">For example, hello following script establishes a **sqlcmd** connection toohello primary replica through hello listener with Windows authentication:</span></span>
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    <span data-ttu-id="95a22-163">Pokud naslouchací proces hello používá port jiný než hello výchozí port (1433), zadejte hello port v hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="95a22-163">If hello listener is using a port other than hello default port (1433), specify hello port in hello connection string.</span></span> <span data-ttu-id="95a22-164">Například hello následujícího příkazu sqlcmd připojuje tooa naslouchání na portu 1435:</span><span class="sxs-lookup"><span data-stu-id="95a22-164">For example, hello following sqlcmd command connects tooa listener at port 1435:</span></span> 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="95a22-165">Hello SQLCMD připojení automaticky připojí toowhichever instanci systému SQL Server hostitele hello primární repliky.</span><span class="sxs-lookup"><span data-stu-id="95a22-165">hello SQLCMD connection automatically connects toowhichever instance of SQL Server hosts hello primary replica.</span></span> 

> [!NOTE]
> <span data-ttu-id="95a22-166">Ujistěte se, že je otevřen v bráně firewall hello obou serverů SQL hello port, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="95a22-166">Make sure that hello port you specify is open on hello firewall of both SQL Servers.</span></span> <span data-ttu-id="95a22-167">Oba servery vyžadují příchozí pravidlo pro hello port TCP, který používáte.</span><span class="sxs-lookup"><span data-stu-id="95a22-167">Both servers require an inbound rule for hello TCP port that you use.</span></span> <span data-ttu-id="95a22-168">V tématu [přidat nebo upravit pravidlo brány Firewall](http://technet.microsoft.com/library/cc753558.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="95a22-168">See [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx) for more information.</span></span> 
> 
> 

## <a name="guidelines-and-limitations"></a><span data-ttu-id="95a22-169">Pokyny a omezení</span><span class="sxs-lookup"><span data-stu-id="95a22-169">Guidelines and limitations</span></span>
<span data-ttu-id="95a22-170">Vezměte na vědomí následující pokyny na naslouchací proces skupiny dostupnosti v Azure pomocí nástroje pro vyrovnávání zatížení pro vnitřní hello:</span><span class="sxs-lookup"><span data-stu-id="95a22-170">Note hello following guidelines on availability group listener in Azure using internal load balancer:</span></span>

* <span data-ttu-id="95a22-171">S nástrojem pro vyrovnávání zatížení pro vnitřní, můžete přístup pouze k naslouchání hello z v rámci hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="95a22-171">With an internal load balancer, you only access hello listener from within hello same virtual network.</span></span>


## <a name="for-more-information"></a><span data-ttu-id="95a22-172">Další informace</span><span class="sxs-lookup"><span data-stu-id="95a22-172">For more information</span></span>
<span data-ttu-id="95a22-173">Další informace najdete v tématu [skupiny dostupnosti nakonfigurujte Always On ve virtuálním počítači Azure ručně](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="95a22-173">For more information, see [Configure Always On availability group in Azure VM manually](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="95a22-174">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="95a22-174">PowerShell cmdlets</span></span>
<span data-ttu-id="95a22-175">Použijte hello následující toocreate rutiny prostředí PowerShell interní nástroj pro virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="95a22-175">Use hello following PowerShell cmdlets toocreate an internal load balancer for Azure virtual machines.</span></span>

* <span data-ttu-id="95a22-176">[Nové AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) vytvoří nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="95a22-176">[New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) creates a load balancer.</span></span> 
* <span data-ttu-id="95a22-177">[Nové AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) vytvoří konfiguraci front-end IP adresy pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="95a22-177">[New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) creates a front-end IP configuration for a load balancer.</span></span> 
* <span data-ttu-id="95a22-178">[Nové AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) vytvoří pravidlo konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="95a22-178">[New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) creates a rule configuration for a load balancer.</span></span> 
* <span data-ttu-id="95a22-179">[Nové AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) vytvoří konfiguraci fondu adres back-end pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="95a22-179">[New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) creates a backend address pool configuration for a load balancer.</span></span> 
* <span data-ttu-id="95a22-180">[Nové AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) vytvoří test konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="95a22-180">[New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) creates a probe configuration for a load balancer.</span></span>
* <span data-ttu-id="95a22-181">[Odebrat AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) Odebere skupinu prostředků Azure pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="95a22-181">[Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) removes a load balancer from an Azure resource group.</span></span>
