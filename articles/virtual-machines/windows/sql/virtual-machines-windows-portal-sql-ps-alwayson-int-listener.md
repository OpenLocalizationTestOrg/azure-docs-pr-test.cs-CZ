---
title: "Konfigurace vždy na naslouchací procesy skupiny dostupnosti – Microsoft Azure | Microsoft Docs"
description: "Naslouchací procesy skupiny dostupnosti nakonfigurujte na modelu Azure Resource Manager, pomocí interní nástroj pro jednu nebo více IP adres."
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
ms.openlocfilehash: 74fa1e4c9cfa608a9a385f3dd82a0599fbcc421c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a><span data-ttu-id="6afbd-103">Nakonfigurovat jeden nebo více vždy na dostupnosti naslouchací procesy skupiny - Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6afbd-103">Configure one or more Always On availability group listeners - Resource Manager</span></span>
<span data-ttu-id="6afbd-104">Toto téma ukazuje, jak:</span><span class="sxs-lookup"><span data-stu-id="6afbd-104">This topic shows how to:</span></span>

* <span data-ttu-id="6afbd-105">Vytvořte interní nástroj pro skupiny dostupnosti systému SQL Server pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6afbd-105">Create an internal load balancer for SQL Server availability groups using PowerShell cmdlets.</span></span>
* <span data-ttu-id="6afbd-106">Přidejte další IP adresy pro vyrovnávání zatížení pro víc než jedné skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="6afbd-106">Add additional IP addresses to a load balancer for more than one availability group.</span></span> 

<span data-ttu-id="6afbd-107">Naslouchací proces skupiny dostupnosti je název virtuální sítě, který klienti připojovat k pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="6afbd-107">An availability group listener is a virtual network name that clients connect to for database access.</span></span> <span data-ttu-id="6afbd-108">Nástroj pro vyrovnávání zatížení na virtuálních počítačích Azure, obsahuje IP adresu pro naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="6afbd-108">On Azure virtual machines, a load balancer holds the IP address for the listener.</span></span> <span data-ttu-id="6afbd-109">Nástroje pro vyrovnávání zatížení směruje provoz do instance systému SQL Server, která naslouchá na portu testu.</span><span class="sxs-lookup"><span data-stu-id="6afbd-109">The load balancer routes traffic to the instance of SQL Server that is listening on the probe port.</span></span> <span data-ttu-id="6afbd-110">Skupiny dostupnosti se obvykle používá interní nástroj.</span><span class="sxs-lookup"><span data-stu-id="6afbd-110">Usually, an availability group uses an internal load balancer.</span></span> <span data-ttu-id="6afbd-111">K nástroji pro vyrovnávání zatížení interní Azure můžete hostovat jednu nebo více IP adres.</span><span class="sxs-lookup"><span data-stu-id="6afbd-111">An Azure internal load balancer can host one or many IP addresses.</span></span> <span data-ttu-id="6afbd-112">Každou IP adresu používá port konkrétní test.</span><span class="sxs-lookup"><span data-stu-id="6afbd-112">Each IP address uses a specific probe port.</span></span> <span data-ttu-id="6afbd-113">Tento dokument ukazuje, jak pomocí prostředí PowerShell vytvořit nástroj pro vyrovnávání zatížení nebo přidejte IP adresy do existující Vyrovnávání zatížení pro skupiny dostupnosti systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6afbd-113">This document shows how to use PowerShell to create a load balancer, or add IP addresses to an existing load balancer for SQL Server availability groups.</span></span> 

<span data-ttu-id="6afbd-114">Umožňuje přiřadit k nástroji pro vyrovnávání zatížení pro vnitřní více IP adres je nové do Azure a je dostupný jenom v modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6afbd-114">The ability to assign multiple IP addresses to an internal load balancer is new to Azure and is only available in Resource Manager model.</span></span> <span data-ttu-id="6afbd-115">Tuto úlohu dokončit, musíte mít skupinu dostupnosti systému SQL Server, který je nasazen na virtuálních počítačích Azure v modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6afbd-115">To complete this task, you need to have a SQL Server availability group deployed on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="6afbd-116">Virtuální počítače systému SQL Server musí patřit do stejné skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="6afbd-116">Both SQL Server virtual machines must belong to the same availability set.</span></span> <span data-ttu-id="6afbd-117">Můžete použít [šablony aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) pro automatické vytvoření skupiny dostupnosti ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6afbd-117">You can use the [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) to automatically create the availability group in Azure Resource Manager.</span></span> <span data-ttu-id="6afbd-118">Tato šablona automaticky vytvoří skupiny dostupnosti, včetně nástroje pro vyrovnávání zatížení pro vnitřní za vás.</span><span class="sxs-lookup"><span data-stu-id="6afbd-118">This template automatically creates the availability group, including the internal load balancer for you.</span></span> <span data-ttu-id="6afbd-119">Pokud dáváte přednost, můžete [ruční konfigurace skupiny dostupnosti Always On](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="6afbd-119">If you prefer, you can [manually configure an Always On availability group](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

<span data-ttu-id="6afbd-120">Toto téma vyžaduje vaše skupiny dostupnosti jsou již nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="6afbd-120">This topic requires that your availability groups are already configured.</span></span>  

<span data-ttu-id="6afbd-121">Související témata:</span><span class="sxs-lookup"><span data-stu-id="6afbd-121">Related topics include:</span></span>

* [<span data-ttu-id="6afbd-122">Konfigurace skupin dostupnosti AlwaysOn na virtuálním počítači Azure (GUI)</span><span class="sxs-lookup"><span data-stu-id="6afbd-122">Configure AlwaysOn Availability Groups in Azure VM (GUI)</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [<span data-ttu-id="6afbd-123">Nakonfigurujte připojení VNet-to-VNet s použitím Azure Resource Manageru a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6afbd-123">Configure a VNet-to-VNet connection by using Azure Resource Manager and PowerShell</span></span>](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a><span data-ttu-id="6afbd-124">Konfigurace brány Windows Firewall</span><span class="sxs-lookup"><span data-stu-id="6afbd-124">Configure the Windows Firewall</span></span>
<span data-ttu-id="6afbd-125">Konfigurace brány Windows Firewall pro povolení přístupu k SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="6afbd-125">Configure the Windows Firewall to allow SQL Server access.</span></span> <span data-ttu-id="6afbd-126">Pravidla brány firewall povolit připojení TCP pro porty používané instance systému SQL Server a kontroly naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="6afbd-126">The firewall rules allow TCP connections to the ports use by the SQL Server instance, and the listener probe.</span></span> <span data-ttu-id="6afbd-127">Podrobné pokyny najdete v tématu [konfigurace brány Windows Firewall pro přístup k databázovému stroji](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="6afbd-127">For detailed instructions, see [Configure a Windows Firewall for Database Engine Access](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1).</span></span> <span data-ttu-id="6afbd-128">Vytvoření příchozího pravidla pro port serveru SQL Server a port testu.</span><span class="sxs-lookup"><span data-stu-id="6afbd-128">Create an inbound rule for the SQL Server port and for the probe port.</span></span>

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a><span data-ttu-id="6afbd-129">Ukázkový skript: Vytvoření Vyrovnávání zatížení interní pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6afbd-129">Example Script: Create an internal load balancer with PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="6afbd-130">Pokud jste vytvořili vaší skupiny dostupnosti s [šablony aplikace Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), nástroje pro vyrovnávání zatížení pro vnitřní již bylo vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="6afbd-130">If you created your availability group with the [Microsoft template](virtual-machines-windows-portal-sql-alwayson-availability-groups.md), the internal load balancer was already created.</span></span> 
> 
> 

<span data-ttu-id="6afbd-131">Následující skript prostředí PowerShell vytvoří interní nástroj, nakonfiguruje pravidla Vyrovnávání zatížení a nastaví IP adresu pro nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-131">The following PowerShell script creates an internal load balancer, configures the load balancing rules, and sets an IP address for the load balancer.</span></span> <span data-ttu-id="6afbd-132">Pokud chcete spustit skript, otevřete Windows PowerShell ISE a vložte skript v podokně skriptu.</span><span class="sxs-lookup"><span data-stu-id="6afbd-132">To run the script, open Windows PowerShell ISE, and paste the script in the Script pane.</span></span> <span data-ttu-id="6afbd-133">Použití `Login-AzureRMAccount` pro přihlášení k prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6afbd-133">Use `Login-AzureRMAccount` to log in to PowerShell.</span></span> <span data-ttu-id="6afbd-134">Pokud máte víc předplatných Azure, použijte `Select-AzureRmSubscription ` nastavte předplatné.</span><span class="sxs-lookup"><span data-stu-id="6afbd-134">If you have multiple Azure subscriptions, use `Select-AzureRmSubscription ` to set the subscription.</span></span> 

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

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the back-end configuration

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

## <span data-ttu-id="6afbd-135"><a name="Add-IP"></a>Ukázkový skript: Přidat IP adresu existující Vyrovnávání zatížení s prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6afbd-135"><a name="Add-IP"></a> Example script: Add an IP address to an existing load balancer with PowerShell</span></span>
<span data-ttu-id="6afbd-136">Pokud chcete použít více než jedné skupiny dostupnosti, přidejte další IP adresu nástroji pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-136">To use more than one availability group, add an additional IP address to the load balancer.</span></span> <span data-ttu-id="6afbd-137">Každou IP adresu vyžaduje vlastní pravidla Vyrovnávání zatížení, port testu a front port.</span><span class="sxs-lookup"><span data-stu-id="6afbd-137">Each IP address requires its own load balancing rule, probe port, and front port.</span></span>

<span data-ttu-id="6afbd-138">Front-end port je port, který aplikace použít pro připojení k instanci systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6afbd-138">The front-end port is the port that applications use to connect to the SQL Server instance.</span></span> <span data-ttu-id="6afbd-139">IP adresy pro skupiny dostupnosti jinou můžete použít stejný port front-endu.</span><span class="sxs-lookup"><span data-stu-id="6afbd-139">IP addresses for different availability groups can use the same front-end port.</span></span>

> [!NOTE]
> <span data-ttu-id="6afbd-140">Pro skupiny dostupnosti systému SQL Server vyžaduje každou IP adresu, port konkrétní testu.</span><span class="sxs-lookup"><span data-stu-id="6afbd-140">For SQL Server availability groups, each IP address requires a specific probe port.</span></span> <span data-ttu-id="6afbd-141">Například pokud jednu IP adresu zařízení na Vyrovnávání zatížení používá port testu 59999, žádné jiné IP adresy na tento nástroj pro vyrovnávání zatížení můžete použít port testu 59999.</span><span class="sxs-lookup"><span data-stu-id="6afbd-141">For example, if one IP address on a load balancer uses probe port 59999, no other IP addresses on that load balancer can use probe port 59999.</span></span>

* <span data-ttu-id="6afbd-142">Informace o omezeních pro vyrovnávání zatížení najdete v tématu **privátní front-endu IP adresu na nástroj pro vyrovnávání zatížení** pod [omezení sítě - Správce Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span><span class="sxs-lookup"><span data-stu-id="6afbd-142">For information about load balancer limits, see **Private front end IP per load balancer** under [Networking Limits - Azure Resource Manager](../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).</span></span>
* <span data-ttu-id="6afbd-143">Informace o omezeních skupiny dostupnosti naleznete v tématu [omezení (skupiny dostupnosti)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span><span class="sxs-lookup"><span data-stu-id="6afbd-143">For information about availability group limits, see [Restrictions (Availability Groups)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).</span></span>

<span data-ttu-id="6afbd-144">Následující skript přidá novou IP adresu do existující pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-144">The following script adds a new IP address to an existing load balancer.</span></span> <span data-ttu-id="6afbd-145">ILB používá port naslouchacího procesu pro front-end port Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-145">The ILB uses the listener port for the load balancing front-end port.</span></span> <span data-ttu-id="6afbd-146">Port, který SQL Server naslouchá na může být tento port.</span><span class="sxs-lookup"><span data-stu-id="6afbd-146">This port can be the port that SQL Server is listening on.</span></span> <span data-ttu-id="6afbd-147">Pro výchozí instance systému SQL Server je port 1433.</span><span class="sxs-lookup"><span data-stu-id="6afbd-147">For default instances of SQL Server, the port is 1433.</span></span> <span data-ttu-id="6afbd-148">Pravidlo pro skupinu dostupnosti Vyrovnávání zatížení vyžaduje plovoucí IP (přímá odpověď ze serveru), tak back-end port je stejný jako front-end port.</span><span class="sxs-lookup"><span data-stu-id="6afbd-148">The load balancing rule for an availability group requires a floating IP (direct server return) so the back-end port is the same as the front-end port.</span></span> <span data-ttu-id="6afbd-149">Aktualizujte proměnné pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="6afbd-149">Update the variables for your environment.</span></span> 

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

## <a name="configure-the-listener"></a><span data-ttu-id="6afbd-150">Konfigurace naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="6afbd-150">Configure the listener</span></span>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a><span data-ttu-id="6afbd-151">Nastavit port naslouchacího procesu v systému SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="6afbd-151">Set the listener port in SQL Server Management Studio</span></span>

1. <span data-ttu-id="6afbd-152">Spusťte SQL Server Management Studio a připojte se k primární replice.</span><span class="sxs-lookup"><span data-stu-id="6afbd-152">Launch SQL Server Management Studio and connect to the primary replica.</span></span>

1. <span data-ttu-id="6afbd-153">Přejděte na **AlwaysOn vysokou dostupnost** | **skupiny dostupnosti** | **naslouchací procesy skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="6afbd-153">Navigate to **AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span> 

1. <span data-ttu-id="6afbd-154">Teď byste měli vidět název naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6afbd-154">You should now see the listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="6afbd-155">Klikněte pravým tlačítkem na název naslouchacího procesu a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6afbd-155">Right-click the listener name and click **Properties**.</span></span>

1. <span data-ttu-id="6afbd-156">V **Port** pole, zadejte číslo portu pro naslouchací proces skupiny dostupnosti pomocí $EndpointPort jste použili předtím (1433 se výchozí nastavení), pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6afbd-156">In the **Port** box, specify the port number for the availability group listener by using the $EndpointPort you used earlier (1433 was the default), then click **OK**.</span></span>

## <a name="test-the-connection-to-the-listener"></a><span data-ttu-id="6afbd-157">Test připojení k naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="6afbd-157">Test the connection to the listener</span></span>

<span data-ttu-id="6afbd-158">K testování připojení:</span><span class="sxs-lookup"><span data-stu-id="6afbd-158">To test the connection:</span></span>

1. <span data-ttu-id="6afbd-159">RDP k systému SQL Server, který je ve stejné virtuální síti, ale není vlastníkem repliky.</span><span class="sxs-lookup"><span data-stu-id="6afbd-159">RDP to a SQL Server that is in the same virtual network, but does not own the replica.</span></span> <span data-ttu-id="6afbd-160">To může být SQL Server v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6afbd-160">This can be the other SQL Server in the cluster.</span></span>

1. <span data-ttu-id="6afbd-161">Použití **sqlcmd** nástroj k testování připojení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-161">Use **sqlcmd** utility to test the connection.</span></span> <span data-ttu-id="6afbd-162">Například následující skript vytvoří **sqlcmd** připojení k primární replice prostřednictvím naslouchací proces s ověřováním systému Windows:</span><span class="sxs-lookup"><span data-stu-id="6afbd-162">For example, the following script establishes a **sqlcmd** connection to the primary replica through the listener with Windows authentication:</span></span>
   
    ```
    sqlmd -S <listenerName> -E
    ```
   
    <span data-ttu-id="6afbd-163">Pokud naslouchací proces používá jiný port než výchozí port (1433), zadejte port v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="6afbd-163">If the listener is using a port other than the default port (1433), specify the port in the connection string.</span></span> <span data-ttu-id="6afbd-164">Například následující příkaz sqlcmd připojí k naslouchání na portu 1435:</span><span class="sxs-lookup"><span data-stu-id="6afbd-164">For example, the following sqlcmd command connects to a listener at port 1435:</span></span> 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="6afbd-165">Připojení SQLCMD se automaticky připojí na kteroukoli instanci systému SQL Server hostitelem primární repliky.</span><span class="sxs-lookup"><span data-stu-id="6afbd-165">The SQLCMD connection automatically connects to whichever instance of SQL Server hosts the primary replica.</span></span> 

> [!NOTE]
> <span data-ttu-id="6afbd-166">Ujistěte se, že je port, který zadáte otevřen v bráně firewall oba servery SQL.</span><span class="sxs-lookup"><span data-stu-id="6afbd-166">Make sure that the port you specify is open on the firewall of both SQL Servers.</span></span> <span data-ttu-id="6afbd-167">Oba servery vyžadují příchozí pravidlo pro port TCP, který používáte.</span><span class="sxs-lookup"><span data-stu-id="6afbd-167">Both servers require an inbound rule for the TCP port that you use.</span></span> <span data-ttu-id="6afbd-168">V tématu [přidat nebo upravit pravidlo brány Firewall](http://technet.microsoft.com/library/cc753558.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6afbd-168">See [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx) for more information.</span></span> 
> 
> 

## <a name="guidelines-and-limitations"></a><span data-ttu-id="6afbd-169">Pokyny a omezení</span><span class="sxs-lookup"><span data-stu-id="6afbd-169">Guidelines and limitations</span></span>
<span data-ttu-id="6afbd-170">Všimněte si, že následující pokyny na naslouchací proces skupiny dostupnosti v Azure pomocí interní nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="6afbd-170">Note the following guidelines on availability group listener in Azure using internal load balancer:</span></span>

* <span data-ttu-id="6afbd-171">S nástrojem pro vyrovnávání zatížení pro vnitřní můžete přístup jenom k naslouchání z v rámci stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="6afbd-171">With an internal load balancer, you only access the listener from within the same virtual network.</span></span>


## <a name="for-more-information"></a><span data-ttu-id="6afbd-172">Další informace</span><span class="sxs-lookup"><span data-stu-id="6afbd-172">For more information</span></span>
<span data-ttu-id="6afbd-173">Další informace najdete v tématu [skupiny dostupnosti nakonfigurujte Always On ve virtuálním počítači Azure ručně](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span><span class="sxs-lookup"><span data-stu-id="6afbd-173">For more information, see [Configure Always On availability group in Azure VM manually](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).</span></span>

## <a name="powershell-cmdlets"></a><span data-ttu-id="6afbd-174">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6afbd-174">PowerShell cmdlets</span></span>
<span data-ttu-id="6afbd-175">Pomocí následující rutiny prostředí PowerShell k vytvoření interní nástroj pro virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6afbd-175">Use the following PowerShell cmdlets to create an internal load balancer for Azure virtual machines.</span></span>

* <span data-ttu-id="6afbd-176">[Nové AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) vytvoří nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-176">[New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) creates a load balancer.</span></span> 
* <span data-ttu-id="6afbd-177">[Nové AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) vytvoří konfiguraci front-end IP adresy pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-177">[New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) creates a front-end IP configuration for a load balancer.</span></span> 
* <span data-ttu-id="6afbd-178">[Nové AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) vytvoří pravidlo konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-178">[New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) creates a rule configuration for a load balancer.</span></span> 
* <span data-ttu-id="6afbd-179">[Nové AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) vytvoří konfiguraci fondu adres back-end pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-179">[New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) creates a backend address pool configuration for a load balancer.</span></span> 
* <span data-ttu-id="6afbd-180">[Nové AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) vytvoří test konfigurace pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-180">[New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) creates a probe configuration for a load balancer.</span></span>
* <span data-ttu-id="6afbd-181">[Odebrat AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) Odebere skupinu prostředků Azure pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6afbd-181">[Remove-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx) removes a load balancer from an Azure resource group.</span></span>
