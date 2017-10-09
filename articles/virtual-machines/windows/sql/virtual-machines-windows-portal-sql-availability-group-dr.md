---
title: "aaaSQL zotavení po havárii skupin dostupnosti serveru – virtuální počítače Azure - | Microsoft Docs"
description: "Tento článek vysvětluje, jak seskupit tooconfigure dostupnosti systému SQL Server na virtuálních počítačích Azure s replikou v jiné oblasti."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: df6238dc61c5a56879c75c9bf7314c618f43c0ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a><span data-ttu-id="c61bf-103">Konfigurace skupiny dostupnosti Always On na virtuálních počítačích, které jsou v různých oblastech Azure</span><span class="sxs-lookup"><span data-stu-id="c61bf-103">Configure an Always On availability group on Azure virtual machines in different regions</span></span>

<span data-ttu-id="c61bf-104">Tento článek vysvětluje, jak skupiny dostupnosti SQL serveru Always On tooconfigure repliky na virtuálních počítačích Azure ve vzdáleném umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="c61bf-104">This article explains how tooconfigure a SQL Server Always On availability group replica on Azure virtual machines in a remote Azure location.</span></span> <span data-ttu-id="c61bf-105">Použijte toto obnovení po havárii toosupport konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c61bf-105">Use this configuration toosupport disaster recovery.</span></span>

<span data-ttu-id="c61bf-106">Tento článek se týká tooAzure virtuální počítače v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c61bf-106">This article applies tooAzure Virtual Machines in Resource Manager mode.</span></span>

<span data-ttu-id="c61bf-107">Hello následující obrázek ukazuje běžné nasazení skupiny dostupnosti na virtuálních počítačích Azure:</span><span class="sxs-lookup"><span data-stu-id="c61bf-107">hello following image shows a common deployment of an availability group on Azure virtual machines:</span></span>

   ![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

<span data-ttu-id="c61bf-109">V tomto nasazení jsou všechny virtuální počítače v jedné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="c61bf-109">In this deployment, all virtual machines are in one Azure region.</span></span> <span data-ttu-id="c61bf-110">repliky skupin dostupnosti Hello může mít synchronním potvrzováním s automatické převzetí služeb při selhání na SQL-1 a SQL 2.</span><span class="sxs-lookup"><span data-stu-id="c61bf-110">hello availability group replicas can have synchronous commit with automatic failover on SQL-1 and SQL-2.</span></span> <span data-ttu-id="c61bf-111">toobuild této architektury, najdete v části [skupiny dostupnosti šablony nebo kurzu](virtual-machines-windows-portal-sql-availability-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c61bf-111">toobuild this architecture, see [Availability Group template or tutorial](virtual-machines-windows-portal-sql-availability-group-overview.md).</span></span>

<span data-ttu-id="c61bf-112">Tato architektura je snadno napadnutelný toodowntime hello oblast Azure přestane být nedostupné.</span><span class="sxs-lookup"><span data-stu-id="c61bf-112">This architecture is vulnerable toodowntime if hello Azure region becomes inaccessible.</span></span> <span data-ttu-id="c61bf-113">tooovercome toto ohrožení zabezpečení, přidejte repliku v jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="c61bf-113">tooovercome this vulnerability, add a replica in a different Azure region.</span></span> <span data-ttu-id="c61bf-114">Hello následující diagram znázorňuje vzhledu nové architektury hello:</span><span class="sxs-lookup"><span data-stu-id="c61bf-114">hello following diagram shows how hello new architecture would look:</span></span>

   ![Zotavení po Havárii skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

<span data-ttu-id="c61bf-116">Hello předchozí diagram ukazuje nový virtuální počítač názvem SQL-3.</span><span class="sxs-lookup"><span data-stu-id="c61bf-116">hello preceding diagram shows a new virtual machine called SQL-3.</span></span> <span data-ttu-id="c61bf-117">SQL 3 je v jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="c61bf-117">SQL-3 is in a different Azure region.</span></span> <span data-ttu-id="c61bf-118">SQL 3 se přidá toohello clusteru převzetí služeb při selhání systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="c61bf-118">SQL-3 is added toohello Windows Server Failover Cluster.</span></span> <span data-ttu-id="c61bf-119">SQL 3 můžete hostovat repliku skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c61bf-119">SQL-3 can host an availability group replica.</span></span> <span data-ttu-id="c61bf-120">Všimněte si, že tento hello oblast Azure SQL 3 má nové nástroje pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="c61bf-120">Finally, notice that hello Azure region for SQL-3 has a new Azure load balancer.</span></span>

>[!NOTE]
> <span data-ttu-id="c61bf-121">Nastavení Azure dostupnosti je potřeba při více než jeden virtuální počítač je v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="c61bf-121">An Azure availability set is required when more than one virtual machine is in hello same region.</span></span> <span data-ttu-id="c61bf-122">Pokud jenom jeden virtuální počítač je v oblasti hello, pak skupinu dostupnosti hello se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="c61bf-122">If only one virtual machine is in hello region, then hello availability set is not required.</span></span> <span data-ttu-id="c61bf-123">Virtuální počítač můžete umístit pouze ve skupině dostupnosti nastavena v okamžiku vytvoření.</span><span class="sxs-lookup"><span data-stu-id="c61bf-123">You can only place a virtual machine in an availability set at creation time.</span></span> <span data-ttu-id="c61bf-124">Pokud už hello virtuální počítač je v nastavení dostupnosti, můžete přidat virtuální počítač pro repliku další později.</span><span class="sxs-lookup"><span data-stu-id="c61bf-124">If hello virtual machine is already in an availability set, you can add a virtual machine for an additional replica later.</span></span>

<span data-ttu-id="c61bf-125">V této architektuře hello repliky v oblasti vzdálené hello je obvykle nakonfigurované asynchronního potvrzování dostupnosti režim a režim ručního převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c61bf-125">In this architecture, hello replica in hello remote region is normally configured with asynchronous commit availability mode and manual failover mode.</span></span>

<span data-ttu-id="c61bf-126">Pokud replik skupin dostupnosti jsou na virtuálních počítačích Azure v různých oblastech Azure, vyžaduje každá oblast:</span><span class="sxs-lookup"><span data-stu-id="c61bf-126">When availability group replicas are on Azure virtual machines in different Azure regions, each region requires:</span></span>

* <span data-ttu-id="c61bf-127">Brána virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c61bf-127">A virtual network gateway</span></span>
* <span data-ttu-id="c61bf-128">Připojení brány virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c61bf-128">A virtual network gateway connection</span></span>

<span data-ttu-id="c61bf-129">Hello následující diagram znázorňuje způsob hello sítě komunikace mezi datovými centry.</span><span class="sxs-lookup"><span data-stu-id="c61bf-129">hello following diagram shows how hello networks communicate between data centers.</span></span>

   ![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
><span data-ttu-id="c61bf-131">Tato architektura způsobuje poplatky za odchozí data pro data replikovat mezi oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="c61bf-131">This architecture incurs outbound data charges for data replicated between Azure regions.</span></span> <span data-ttu-id="c61bf-132">V tématu [šířky pásma ceny](http://azure.microsoft.com/pricing/details/bandwidth/).</span><span class="sxs-lookup"><span data-stu-id="c61bf-132">See [Bandwidth Pricing](http://azure.microsoft.com/pricing/details/bandwidth/).</span></span>  

## <a name="create-remote-replica"></a><span data-ttu-id="c61bf-133">Vytvoření vzdálené repliky</span><span class="sxs-lookup"><span data-stu-id="c61bf-133">Create remote replica</span></span>

<span data-ttu-id="c61bf-134">toocreate repliku v centru vzdálených dat, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c61bf-134">toocreate a replica in a remote data center, do hello following steps:</span></span>

1. <span data-ttu-id="c61bf-135">[Vytvořit virtuální síť v oblasti nové hello](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="c61bf-135">[Create a virtual network in hello new region](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>

1. <span data-ttu-id="c61bf-136">[Konfigurace připojení typu VNet-to-VNet pomocí portálu Azure hello](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c61bf-136">[Configure a VNet-to-VNet connection using hello Azure portal](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).</span></span>

   >[!NOTE]
   ><span data-ttu-id="c61bf-137">V některých případech může mít toouse prostředí PowerShell toocreate hello VNet-to-VNet připojení.</span><span class="sxs-lookup"><span data-stu-id="c61bf-137">In some cases, you may have toouse PowerShell toocreate hello VNet-to-VNet connection.</span></span> <span data-ttu-id="c61bf-138">Například pokud použijete různé účty Azure nelze nakonfigurovat připojení hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="c61bf-138">For example, if you use different Azure accounts you cannot configure hello connection in hello portal.</span></span> <span data-ttu-id="c61bf-139">V takovém případě najdete [konfigurace VNet-to-VNet připojení pomocí portálu Azure hello](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c61bf-139">In this case see, [Configure a VNet-to-VNet connection using hello Azure portal](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

1. <span data-ttu-id="c61bf-140">[Vytvoření řadiče domény v nové oblasti hello](../../../active-directory/active-directory-new-forest-virtual-machine.md).</span><span class="sxs-lookup"><span data-stu-id="c61bf-140">[Create a domain controller in hello new region](../../../active-directory/active-directory-new-forest-virtual-machine.md).</span></span>

   <span data-ttu-id="c61bf-141">Tento řadič domény poskytuje ověřování, pokud není dostupný řadič domény hello v primární lokalitě hello.</span><span class="sxs-lookup"><span data-stu-id="c61bf-141">This domain controller provides authentication if hello domain controller in hello primary site is not available.</span></span>

1. <span data-ttu-id="c61bf-142">[Vytvoření virtuálního počítače s SQL serverem v nové oblasti hello](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="c61bf-142">[Create a SQL Server virtual machine in hello new region](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

1. <span data-ttu-id="c61bf-143">[Vytvoření pro vyrovnávání zatížení Azure v síti hello na novou oblast hello](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="c61bf-143">[Create an Azure load balancer in hello network on hello new region](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span></span>

   <span data-ttu-id="c61bf-144">Tento nástroj pro vyrovnávání zatížení musí:</span><span class="sxs-lookup"><span data-stu-id="c61bf-144">This load balancer must:</span></span>

   - <span data-ttu-id="c61bf-145">V hello stejnou síť a podsíť, protože hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c61bf-145">Be in hello same network and subnet as hello new virtual machine.</span></span>
   - <span data-ttu-id="c61bf-146">Máte statickou IP adresu pro naslouchací proces skupiny dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="c61bf-146">Have a static IP address for hello availability group listener.</span></span>
   - <span data-ttu-id="c61bf-147">Zahrnout back-endový fond, který se skládá z jen hello virtuálních počítačů v hello stejné oblasti jako hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c61bf-147">Include a backend pool consisting of only hello virtual machines in hello same region as hello load balancer.</span></span>
   - <span data-ttu-id="c61bf-148">Použijte TCP port testu toohello konkrétní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c61bf-148">Use a TCP port probe specific toohello IP address.</span></span>
   - <span data-ttu-id="c61bf-149">Pravidlo konkrétní toohello systému SQL Server v hello Vyrovnávání zatížení mají stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="c61bf-149">Have a load balancing rule specific toohello SQL Server in hello same region.</span></span>  

1. <span data-ttu-id="c61bf-150">[Přidat Clustering převzetí služeb při selhání funkce toohello nový Server SQL](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span><span class="sxs-lookup"><span data-stu-id="c61bf-150">[Add Failover Clustering feature toohello new SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span></span>

1. <span data-ttu-id="c61bf-151">[Připojit hello nové doméně systému SQL Server toohello](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span><span class="sxs-lookup"><span data-stu-id="c61bf-151">[Join hello new SQL Server toohello domain](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span></span>

1. <span data-ttu-id="c61bf-152">[Nastavit účet domény hello nového systému SQL Server service účet toouse](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).</span><span class="sxs-lookup"><span data-stu-id="c61bf-152">[Set hello new SQL Server service account toouse a domain account](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).</span></span>

1. <span data-ttu-id="c61bf-153">[Přidání nového serveru SQL Server toohello hello clusteru převzetí služeb při selhání systému Windows Server](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).</span><span class="sxs-lookup"><span data-stu-id="c61bf-153">[Add hello new SQL Server toohello Windows Server Failover Cluster](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).</span></span>

1. <span data-ttu-id="c61bf-154">Vytvořte prostředek adresy IP na hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c61bf-154">Create an IP address resource on hello cluster.</span></span>

   <span data-ttu-id="c61bf-155">Ve Správci clusteru převzetí služeb při selhání můžete vytvořit prostředek hello IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c61bf-155">You can create hello IP address resource in Failover Cluster Manager.</span></span> <span data-ttu-id="c61bf-156">Klikněte pravým tlačítkem na hello role skupiny dostupnosti, klikněte na tlačítko **přidat prostředek**, **více prostředků**a klikněte na tlačítko **IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="c61bf-156">Right-click hello availability group role, click **Add Resource**, **More Resources**, and click **IP Address**.</span></span>

   ![Vytvoření IP adresy](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   <span data-ttu-id="c61bf-158">Tato IP adresa nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c61bf-158">Configure this IP address as follows:</span></span>

   - <span data-ttu-id="c61bf-159">Použijte síť hello z hello vzdáleného datového centra.</span><span class="sxs-lookup"><span data-stu-id="c61bf-159">Use hello network from hello remote data center.</span></span>
   - <span data-ttu-id="c61bf-160">Přiřadíte hello IP adresu z hello nové pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="c61bf-160">Assign hello IP address from hello new Azure load balancer.</span></span> 

1. <span data-ttu-id="c61bf-161">Na hello nový Server SQL v SQL Server Configuration Manager [povolte skupiny dostupnosti Always On](http://msdn.microsoft.com/library/ff878259.aspx).</span><span class="sxs-lookup"><span data-stu-id="c61bf-161">On hello new SQL Server in SQL Server Configuration Manager, [enable Always On Availability Groups](http://msdn.microsoft.com/library/ff878259.aspx).</span></span>

1. <span data-ttu-id="c61bf-162">[Porty brány firewall otevřít na hello nový Server SQL](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span><span class="sxs-lookup"><span data-stu-id="c61bf-162">[Open firewall ports on hello new SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span></span>

   <span data-ttu-id="c61bf-163">je třeba tooopen čísla portů Hello závisí na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="c61bf-163">hello port numbers you need tooopen depend on your environment.</span></span> <span data-ttu-id="c61bf-164">Otevřené porty pro hello zrcadlení koncový bod a Azure načíst sondu nástroje pro vyrovnávání stavu.</span><span class="sxs-lookup"><span data-stu-id="c61bf-164">Open ports for hello mirroring endpoint and Azure load balancer health probe.</span></span>

1. <span data-ttu-id="c61bf-165">[Přidejte skupinu dostupnosti toohello repliky na hello nový Server SQL](http://msdn.microsoft.com/library/hh213239.aspx).</span><span class="sxs-lookup"><span data-stu-id="c61bf-165">[Add a replica toohello availability group on hello new SQL Server](http://msdn.microsoft.com/library/hh213239.aspx).</span></span>

   <span data-ttu-id="c61bf-166">Pro repliku ve vzdálené oblast Azure nastavte ji pro asynchronní replikaci s ruční převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c61bf-166">For a replica in a remote Azure region, set it for asynchronous replication with manual failover.</span></span>  

1. <span data-ttu-id="c61bf-167">Přidáte prostředek hello IP adresy jako závislost pro hello naslouchací proces klientský přístup k bodu (síťový název) clusteru.</span><span class="sxs-lookup"><span data-stu-id="c61bf-167">Add hello IP address resource as a dependency for hello listener client access point (network name) cluster.</span></span>

   <span data-ttu-id="c61bf-168">Hello následující snímek obrazovky ukazuje prostředek clusteru správně nakonfigurovaná adresa IP:</span><span class="sxs-lookup"><span data-stu-id="c61bf-168">hello following screenshot shows a properly configured IP address cluster resource:</span></span>

   ![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   ><span data-ttu-id="c61bf-170">skupinu prostředků clusteru Hello zahrnuje obě IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c61bf-170">hello cluster resource group includes both IP addresses.</span></span> <span data-ttu-id="c61bf-171">Obě IP adresy jsou závislosti pro hello naslouchací proces klientský přístupový bod.</span><span class="sxs-lookup"><span data-stu-id="c61bf-171">Both IP addresses are dependencies for hello listener client access point.</span></span> <span data-ttu-id="c61bf-172">Použití hello **nebo** operátor v konfiguraci závislostí hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c61bf-172">Use hello **OR** operator in hello cluster dependency configuration.</span></span>

1. <span data-ttu-id="c61bf-173">[Nastavení parametrů clusteru hello v prostředí PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).</span><span class="sxs-lookup"><span data-stu-id="c61bf-173">[Set hello cluster parameters in PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).</span></span>

<span data-ttu-id="c61bf-174">Spusťte skript prostředí PowerShell hello s hello název sítě s clustery, IP adresa a port testu, který jste nakonfigurovali na Vyrovnávání zatížení hello v nové oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="c61bf-174">Run hello PowerShell script with hello cluster network name, IP address, and probe port that you configured on hello load balancer in hello new region.</span></span>

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster name for hello network in hello new region (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "<IPResourceName>" # hello cluster name for hello new IP Address resource.
   $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB) in hello new region. This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <nnnnn> # hello probe port you set on hello ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a><span data-ttu-id="c61bf-175">Sada připojení pro více podsítí</span><span class="sxs-lookup"><span data-stu-id="c61bf-175">Set connection for multiple subnets</span></span>

<span data-ttu-id="c61bf-176">replika Hello v hello vzdáleného datového centra je součástí skupiny dostupnosti hello, ale je v jiné podsíti.</span><span class="sxs-lookup"><span data-stu-id="c61bf-176">hello replica in hello remote data center is part of hello availability group but it is in a different subnet.</span></span> <span data-ttu-id="c61bf-177">Pokud tato replika bude hello primární replikou, může dojít, překročení časového limitu připojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c61bf-177">If this replica becomes hello primary replica, application connection time-outs may occur.</span></span> <span data-ttu-id="c61bf-178">Toto chování je hello stejné jako místní skupiny dostupnosti v nasazení více podsítí.</span><span class="sxs-lookup"><span data-stu-id="c61bf-178">This behavior is hello same as an on-premises availability group in a multi-subnet deployment.</span></span> <span data-ttu-id="c61bf-179">tooallow připojení z klientské aplikace, aktualizace připojení klienta hello nebo nakonfigurovat překlad ukládání do mezipaměti na prostředku názvu sítě clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c61bf-179">tooallow connections from client applications, either update hello client connection or configure name resolution caching on hello cluster network name resource.</span></span>

<span data-ttu-id="c61bf-180">Pokud možno aktualizovat tooset řetězce připojení klienta hello `MultiSubnetFailover=Yes`.</span><span class="sxs-lookup"><span data-stu-id="c61bf-180">Preferably, update hello client connection strings tooset `MultiSubnetFailover=Yes`.</span></span> <span data-ttu-id="c61bf-181">V tématu [propojení s MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="c61bf-181">See [Connecting With MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).</span></span>

<span data-ttu-id="c61bf-182">Pokud nelze upravit hello připojovací řetězce, můžete nakonfigurovat název řešení ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c61bf-182">If you cannot modify hello connection strings, you can configure name resolution caching.</span></span> <span data-ttu-id="c61bf-183">V tématu [časových limitů připojení ve skupině dostupnosti více podsítí](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).</span><span class="sxs-lookup"><span data-stu-id="c61bf-183">See [Connection Timeouts in Multi-subnet Availability Group](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).</span></span>

## <a name="fail-over-tooremote-region"></a><span data-ttu-id="c61bf-184">Selhání tooremote oblast</span><span class="sxs-lookup"><span data-stu-id="c61bf-184">Fail over tooremote region</span></span>

<span data-ttu-id="c61bf-185">tootest naslouchací proces připojení toohello oblasti vzdáleného, můžete převzít hello repliky toohello vzdálené oblasti.</span><span class="sxs-lookup"><span data-stu-id="c61bf-185">tootest listener connectivity toohello remote region, you can fail over hello replica toohello remote region.</span></span> <span data-ttu-id="c61bf-186">Replika hello je asynchronní, i když je převzetí služeb při selhání ztráty dat snadno napadnutelný toopotential.</span><span class="sxs-lookup"><span data-stu-id="c61bf-186">While hello replica is asynchronous, failover is vulnerable toopotential data loss.</span></span> <span data-ttu-id="c61bf-187">toofail přes bez ztráty dat, změňte toosynchronous režim dostupnosti hello a nastavte tooautomatic režimu hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c61bf-187">toofail over without data loss, change hello availability mode toosynchronous and set hello failover mode tooautomatic.</span></span> <span data-ttu-id="c61bf-188">Použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c61bf-188">Use hello following steps:</span></span>

1. <span data-ttu-id="c61bf-189">V **Průzkumník objektů**, připojte toohello instanci systému SQL Server, který je hostitelem primární repliky hello.</span><span class="sxs-lookup"><span data-stu-id="c61bf-189">In **Object Explorer**, connect toohello instance of SQL Server that hosts hello primary replica.</span></span>
1. <span data-ttu-id="c61bf-190">V části **skupiny dostupnosti AlwaysOn**, **skupiny dostupnosti**, klikněte pravým tlačítkem na vaší skupiny dostupnosti a klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c61bf-190">Under **AlwaysOn Availability Groups**, **Availability Groups**, right-click your availability group and click **Properties**.</span></span>
1. <span data-ttu-id="c61bf-191">Na hello **Obecné** v části **replik dostupnosti**, sada hello sekundární replika v hello zotavení po Havárii lokality toouse **synchronní potvrdit** režim dostupnosti a **Automatické** režim převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c61bf-191">On hello **General** page, under **Availability Replicas**, set hello secondary replica in hello DR site toouse **Synchronous Commit** availability mode and **Automatic** failover mode.</span></span>
1. <span data-ttu-id="c61bf-192">Pokud máte sekundární repliku ve stejné lokalitě jako váš primární repliky pro vysokou dostupnost, nastavte tuto repliku příliš**asynchronní potvrdit** a **ruční**.</span><span class="sxs-lookup"><span data-stu-id="c61bf-192">If you have a secondary replica in same site as your primary replica for high availability, set this replica too**Asynchronous Commit** and **Manual**.</span></span>
1. <span data-ttu-id="c61bf-193">Klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="c61bf-193">Click OK.</span></span>
1. <span data-ttu-id="c61bf-194">V **Průzkumník objektů**, klikněte pravým tlačítkem na skupinu dostupnosti hello a klikněte na tlačítko **zobrazit řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="c61bf-194">In **Object Explorer**, right-click hello availability group, and click **Show Dashboard**.</span></span>
1. <span data-ttu-id="c61bf-195">Na řídicím panelu hello, ověřte, že hello synchronizaci repliky v lokalitě hello zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="c61bf-195">On hello dashboard, verify that hello replica on hello DR site is synchronized.</span></span>
1. <span data-ttu-id="c61bf-196">V **Průzkumník objektů**, klikněte pravým tlačítkem na skupinu dostupnosti hello a klikněte na tlačítko **převzetí služeb při selhání...** . SQL Server Management studia otevře Průvodce toofail přes SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c61bf-196">In **Object Explorer**, right-click hello availability group, and click **Failover...**. SQL Server Management Studios opens a wizard toofail over SQL Server.</span></span>  
1. <span data-ttu-id="c61bf-197">Klikněte na tlačítko **Další**a vyberte hello instance systému SQL Server v lokalitě hello zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="c61bf-197">Click **Next**, and select hello SQL Server instance in hello DR site.</span></span> <span data-ttu-id="c61bf-198">Klikněte na tlačítko **Další** znovu.</span><span class="sxs-lookup"><span data-stu-id="c61bf-198">Click **Next** again.</span></span>
1. <span data-ttu-id="c61bf-199">Toohello instance systému SQL Server v lokalitě hello zotavení po Havárii a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c61bf-199">Connect toohello SQL Server instance in hello DR site and click **Next**.</span></span>
1. <span data-ttu-id="c61bf-200">Na hello **Souhrn** , ověřte nastavení hello a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c61bf-200">On hello **Summary** page, verify hello settings and click **Finish**.</span></span>

<span data-ttu-id="c61bf-201">Poté, co testování připojení, přesuňte primární data zpět tooyour hello primární repliky na střed a nastavení hello dostupnosti režimu back tootheir normální provozní.</span><span class="sxs-lookup"><span data-stu-id="c61bf-201">After testing connectivity, move hello primary replica back tooyour primary data center and set hello availability mode back tootheir normal operating settings.</span></span> <span data-ttu-id="c61bf-202">Hello následující tabulka uvádí hello normální provozní nastavení hello architektury popsané v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="c61bf-202">hello following table shows hello normal operational settings for hello architecture described in this document:</span></span>

| <span data-ttu-id="c61bf-203">Umístění</span><span class="sxs-lookup"><span data-stu-id="c61bf-203">Location</span></span> | <span data-ttu-id="c61bf-204">Instance serveru</span><span class="sxs-lookup"><span data-stu-id="c61bf-204">Server Instance</span></span> | <span data-ttu-id="c61bf-205">Role</span><span class="sxs-lookup"><span data-stu-id="c61bf-205">Role</span></span> | <span data-ttu-id="c61bf-206">Režim dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c61bf-206">Availability Mode</span></span> | <span data-ttu-id="c61bf-207">Režim převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c61bf-207">Failover Mode</span></span>
| ----- | ----- | ----- | ----- | -----
| <span data-ttu-id="c61bf-208">Primární datové centrum</span><span class="sxs-lookup"><span data-stu-id="c61bf-208">Primary data center</span></span> | <span data-ttu-id="c61bf-209">SQL-1</span><span class="sxs-lookup"><span data-stu-id="c61bf-209">SQL-1</span></span> | <span data-ttu-id="c61bf-210">Primární</span><span class="sxs-lookup"><span data-stu-id="c61bf-210">Primary</span></span> | <span data-ttu-id="c61bf-211">Synchronní</span><span class="sxs-lookup"><span data-stu-id="c61bf-211">Synchronous</span></span> | <span data-ttu-id="c61bf-212">Automatické</span><span class="sxs-lookup"><span data-stu-id="c61bf-212">Automatic</span></span>
| <span data-ttu-id="c61bf-213">Primární datové centrum</span><span class="sxs-lookup"><span data-stu-id="c61bf-213">Primary data center</span></span> | <span data-ttu-id="c61bf-214">SQL-2</span><span class="sxs-lookup"><span data-stu-id="c61bf-214">SQL-2</span></span> | <span data-ttu-id="c61bf-215">Sekundární</span><span class="sxs-lookup"><span data-stu-id="c61bf-215">Secondary</span></span> | <span data-ttu-id="c61bf-216">Synchronní</span><span class="sxs-lookup"><span data-stu-id="c61bf-216">Synchronous</span></span> | <span data-ttu-id="c61bf-217">Automatické</span><span class="sxs-lookup"><span data-stu-id="c61bf-217">Automatic</span></span>
| <span data-ttu-id="c61bf-218">Sekundární nebo vzdáleného datového centra</span><span class="sxs-lookup"><span data-stu-id="c61bf-218">Secondary or remote data center</span></span> | <span data-ttu-id="c61bf-219">SQL – 3</span><span class="sxs-lookup"><span data-stu-id="c61bf-219">SQL-3</span></span> | <span data-ttu-id="c61bf-220">Sekundární</span><span class="sxs-lookup"><span data-stu-id="c61bf-220">Secondary</span></span> | <span data-ttu-id="c61bf-221">Asynchronní</span><span class="sxs-lookup"><span data-stu-id="c61bf-221">Asynchronous</span></span> | <span data-ttu-id="c61bf-222">Ruční</span><span class="sxs-lookup"><span data-stu-id="c61bf-222">Manual</span></span>


### <a name="more-information-about-planned-and-forced-manual-failover"></a><span data-ttu-id="c61bf-223">Další informace o plánované a vynucené ruční převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c61bf-223">More information about planned and forced manual failover</span></span>

<span data-ttu-id="c61bf-224">Další informace najdete v tématu hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="c61bf-224">For more information, see hello following topics:</span></span>

- [<span data-ttu-id="c61bf-225">Provedení plánované ruční převzetí služeb při selhání skupiny dostupnosti (SQL Server)</span><span class="sxs-lookup"><span data-stu-id="c61bf-225">Perform a Planned Manual Failover of an Availability Group (SQL Server)</span></span>](http://msdn.microsoft.com/library/hh231018.aspx)
- [<span data-ttu-id="c61bf-226">Provedení vynucené ruční převzetí služeb při selhání skupiny dostupnosti (SQL Server)</span><span class="sxs-lookup"><span data-stu-id="c61bf-226">Perform a Forced Manual Failover of an Availability Group (SQL Server)</span></span>](http://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a><span data-ttu-id="c61bf-227">Další odkazy</span><span class="sxs-lookup"><span data-stu-id="c61bf-227">Additional Links</span></span>

* [<span data-ttu-id="c61bf-228">Always On skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c61bf-228">Always On Availability Groups</span></span>](http://msdn.microsoft.com/library/hh510230.aspx)
* [<span data-ttu-id="c61bf-229">Virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="c61bf-229">Azure Virtual Machines</span></span>](http://docs.microsoft.com/azure/virtual-machines/windows/)
* [<span data-ttu-id="c61bf-230">Nástroje pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="c61bf-230">Azure Load Balancers</span></span>](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [<span data-ttu-id="c61bf-231">Skupiny dostupnosti Azure</span><span class="sxs-lookup"><span data-stu-id="c61bf-231">Azure Availability Sets</span></span>](../manage-availability.md)
