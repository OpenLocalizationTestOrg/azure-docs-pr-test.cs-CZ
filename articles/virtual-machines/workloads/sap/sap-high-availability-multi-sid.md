---
title: "Vytvoření konfigurace aplikace SAP více SID v Azure | Microsoft Docs"
description: "Průvodce konfigurace více SID SAP NetWeaver s vysokou dostupností v systému Windows, virtuální počítače"
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b48df78df9f53ac7bf0804f55a8d36a2fe2f86b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a><span data-ttu-id="28674-103">Vytvoření konfigurace aplikace SAP NetWeaver více SID</span><span class="sxs-lookup"><span data-stu-id="28674-103">Create an SAP NetWeaver multi-SID configuration</span></span>

[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[sap-ha-guide]:sap-high-availability-guide.md 
[sap-ha-guide-figure-6001]:./media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:./media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:./media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:./media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:./media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png
[networking-limits-azure-resource-manager]:../../../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[sap-ha-guide-9.1.1]:sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 
[sap-ha-guide-8.8]:sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.12.3.3]:sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 
[sap-ha-guide-9]:sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 
[sap-ha-guide-9.1.2]:sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 
[sap-ha-guide-9.1.3]:sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 
[sap-ha-guide-9.1.4]:sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 
[sap-ha-guide-9.4]:sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a 
[sap-ha-guide-9.5]:sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 
[sap-ha-guide-9.6]:sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 
[sap-ha-guide-10]:sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9

<span data-ttu-id="28674-104">V září 2016 společnost Microsoft vydala funkce, kde můžete spravovat pomocí pro vyrovnávání zatížení Azure interní víc virtuálních IP adres.</span><span class="sxs-lookup"><span data-stu-id="28674-104">In September 2016, Microsoft released a feature where you can manage multiple virtual IP addresses by using an Azure internal load balancer.</span></span> <span data-ttu-id="28674-105">Tato funkce již existuje v Azure externím vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="28674-105">This functionality already exists in the Azure external load balancer.</span></span>

<span data-ttu-id="28674-106">Pokud máte nasazení SAP, jak je uvedeno v, můžete použít interní nástroj pro vytvoření konfigurace clusteru systému Windows pro SAP ASC nebo SCS [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide].</span><span class="sxs-lookup"><span data-stu-id="28674-106">If you have an SAP deployment, you can use an internal load balancer to create a Windows cluster configuration for SAP ASCS/SCS, as documented in the [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide].</span></span>

<span data-ttu-id="28674-107">Tento článek se zaměřuje na postup přesunutí z jedné instalace ASC nebo SCS ke konfiguraci více SID SAP nainstalováním další instance SAP ASC nebo SCS clusteru do existujícího clusteru Windows Server Failover Clustering (WSFC).</span><span class="sxs-lookup"><span data-stu-id="28674-107">This article focuses on how to move from a single ASCS/SCS installation to an SAP multi-SID configuration by installing additional SAP ASCS/SCS clustered instances into an existing Windows Server Failover Clustering (WSFC) cluster.</span></span> <span data-ttu-id="28674-108">Po dokončení tohoto procesu jste nakonfigurovali clusteru více SID služby SAP.</span><span class="sxs-lookup"><span data-stu-id="28674-108">When this process is completed, you will have configured an SAP multi-SID cluster.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="28674-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="28674-109">Prerequisites</span></span>
<span data-ttu-id="28674-110">Jste již nakonfigurovali cluster služby WSFC, který se používá pro jednu instanci SAP ASC nebo SCS, jak je popsáno v [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows] [ sap-ha-guide] a jak je znázorněno v tomto diagramu.</span><span class="sxs-lookup"><span data-stu-id="28674-110">You have already configured a WSFC cluster that is used for one SAP ASCS/SCS instance, as discussed in the [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide] and as shown in this diagram.</span></span>

![Instance SAP ASC nebo SCS vysokou dostupnost][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a><span data-ttu-id="28674-112">Cílová architektura</span><span class="sxs-lookup"><span data-stu-id="28674-112">Target architecture</span></span>

<span data-ttu-id="28674-113">Cílem je nainstalovat více ASC ABAP SAP nebo SAP Java SCS Clusterované instance ve stejném clusteru služby WSFC, jako ilustrované tady:</span><span class="sxs-lookup"><span data-stu-id="28674-113">The goal is to install multiple SAP ABAP ASCS or SAP Java SCS clustered instances in the same WSFC cluster, as illustrated here:</span></span>

![Více instancí SAP ASC nebo SCS clusteru v Azure][sap-ha-guide-figure-6002]

> [!NOTE]
><span data-ttu-id="28674-115">Existuje omezení počtu privátní front-end IP adresy pro každý nástroj pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="28674-115">There is a limit to the number of private front-end IPs for each Azure internal load balancer.</span></span>
>
><span data-ttu-id="28674-116">Maximální počet instancí SAP ASC nebo SCS do jednoho clusteru služby WSFC se rovná maximální počet privátní front-end IP adresy pro každý nástroj pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="28674-116">The maximum number of SAP ASCS/SCS instances in one WSFC cluster is equal to the maximum number of private front-end IPs for each Azure internal load balancer.</span></span>
>

<span data-ttu-id="28674-117">Další informace o omezeních pro vyrovnávání zatížení, najdete v části "privátní front-end IP adresy na nástroj pro vyrovnávání zatížení" v [omezení sítě: Azure Resource Manager][networking-limits-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="28674-117">For more information about load-balancer limits, see "Private front end IP per load balancer" in [Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager].</span></span>

<span data-ttu-id="28674-118">Dokončení na šířku s dvěma systémy SAP vysoké dostupnosti bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="28674-118">The complete landscape with two high-availability SAP systems would look like this:</span></span>

![Nastavení vysoké dostupnosti více SID SAP s dvě systému SAP identifikátorů SID][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> <span data-ttu-id="28674-120">Nastavení musí splňovat následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="28674-120">The setup must meet the following conditions:</span></span>
> - <span data-ttu-id="28674-121">Instance SAP ASC nebo SCS musejí sdílet stejný cluster služby WSFC.</span><span class="sxs-lookup"><span data-stu-id="28674-121">The SAP ASCS/SCS instances must share the same WSFC cluster.</span></span>
> - <span data-ttu-id="28674-122">Každý SID databázového systému musí mít svůj vlastní vyhrazený cluster služby WSFC.</span><span class="sxs-lookup"><span data-stu-id="28674-122">Each DBMS SID must have its own dedicated WSFC cluster.</span></span>
> - <span data-ttu-id="28674-123">SAP aplikační servery, které patří do jednoho systému SAP SID musí mít vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="28674-123">SAP application servers that belong to one SAP system SID must have their own dedicated VMs.</span></span>


## <a name="prepare-the-infrastructure"></a><span data-ttu-id="28674-124">Příprava infrastruktury</span><span class="sxs-lookup"><span data-stu-id="28674-124">Prepare the infrastructure</span></span>
<span data-ttu-id="28674-125">Příprava infrastruktury, můžete nainstalovat další instance SAP ASC nebo SCS s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="28674-125">To prepare your infrastructure, you can install an additional SAP ASCS/SCS instance with the following parameters:</span></span>

| <span data-ttu-id="28674-126">Název parametru</span><span class="sxs-lookup"><span data-stu-id="28674-126">Parameter name</span></span> | <span data-ttu-id="28674-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="28674-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="28674-128">SAP ASC NEBO SCS SID</span><span class="sxs-lookup"><span data-stu-id="28674-128">SAP ASCS/SCS SID</span></span> |<span data-ttu-id="28674-129">PR1-lb ASC</span><span class="sxs-lookup"><span data-stu-id="28674-129">pr1-lb-ascs</span></span> |
| <span data-ttu-id="28674-130">Databázového systému SAP interní nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="28674-130">SAP DBMS internal load balancer</span></span> | <span data-ttu-id="28674-131">PR5</span><span class="sxs-lookup"><span data-stu-id="28674-131">PR5</span></span> |
| <span data-ttu-id="28674-132">Název virtuálního hostitele SAP</span><span class="sxs-lookup"><span data-stu-id="28674-132">SAP virtual host name</span></span> | <span data-ttu-id="28674-133">pr5. sap cl</span><span class="sxs-lookup"><span data-stu-id="28674-133">pr5-sap-cl</span></span> |
| <span data-ttu-id="28674-134">SAP ASC nebo SCS virtuální hostitele IP adresu (IP adresa služby Vyrovnávání zatížení Další Azure)</span><span class="sxs-lookup"><span data-stu-id="28674-134">SAP ASCS/SCS virtual host IP address (additional Azure load balancer IP address)</span></span> | <span data-ttu-id="28674-135">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="28674-135">10.0.0.50</span></span> |
| <span data-ttu-id="28674-136">Čísla instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="28674-136">SAP ASCS/SCS instance number</span></span> | <span data-ttu-id="28674-137">50</span><span class="sxs-lookup"><span data-stu-id="28674-137">50</span></span> |
| <span data-ttu-id="28674-138">Port testu ILB pro další instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="28674-138">ILB probe port for additional SAP ASCS/SCS instance</span></span> | <span data-ttu-id="28674-139">62350</span><span class="sxs-lookup"><span data-stu-id="28674-139">62350</span></span> |

> [!NOTE]
> <span data-ttu-id="28674-140">Každou IP adresu pro SAP ASC nebo SCS instance clusteru, vyžaduje port jedinečný testu.</span><span class="sxs-lookup"><span data-stu-id="28674-140">For SAP ASCS/SCS cluster instances, each IP address requires a unique probe port.</span></span> <span data-ttu-id="28674-141">Například pokud jednu IP adresu na Azure interní nástroj používá port testu 62300, žádné jiné IP adresy na tento nástroj pro vyrovnávání zatížení můžete použít port testu 62300.</span><span class="sxs-lookup"><span data-stu-id="28674-141">For example, if one IP address on an Azure internal load balancer uses probe port 62300, no other IP address on that load balancer can use probe port 62300.</span></span>
>
><span data-ttu-id="28674-142">Pro naše účely protože port testu 62300 je rezervovaná, se používá port testu 62350.</span><span class="sxs-lookup"><span data-stu-id="28674-142">For our purposes, because probe port 62300 is already reserved, we are using probe port 62350.</span></span>

<span data-ttu-id="28674-143">V existujícím clusteru služby WSFC s dvěma uzly můžete nainstalovat další instance SAP ASC nebo SCS:</span><span class="sxs-lookup"><span data-stu-id="28674-143">You can install additional SAP ASCS/SCS instances in the existing WSFC cluster with two nodes:</span></span>

| <span data-ttu-id="28674-144">Role virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="28674-144">Virtual machine role</span></span> | <span data-ttu-id="28674-145">Název hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="28674-145">Virtual machine host name</span></span> | <span data-ttu-id="28674-146">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="28674-146">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="28674-147">1. uzel clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="28674-147">1st cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="28674-148">PR1-ASC-0</span><span class="sxs-lookup"><span data-stu-id="28674-148">pr1-ascs-0</span></span> |<span data-ttu-id="28674-149">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="28674-149">10.0.0.10</span></span> |
| <span data-ttu-id="28674-150">2. uzel clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="28674-150">2nd cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="28674-151">PR1-ASC-1</span><span class="sxs-lookup"><span data-stu-id="28674-151">pr1-ascs-1</span></span> |<span data-ttu-id="28674-152">10.0.0.9</span><span class="sxs-lookup"><span data-stu-id="28674-152">10.0.0.9</span></span> |

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance-on-the-dns-server"></a><span data-ttu-id="28674-153">Vytvořte název virtuálního hostitele pro skupinu prostředků clusteru SAP ASC nebo SCS na serveru DNS</span><span class="sxs-lookup"><span data-stu-id="28674-153">Create a virtual host name for the clustered SAP ASCS/SCS instance on the DNS server</span></span>

<span data-ttu-id="28674-154">Položku DNS pro název virtuálního hostitele instance ASC nebo SCS můžete vytvořit pomocí následujících parametrů:</span><span class="sxs-lookup"><span data-stu-id="28674-154">You can create a DNS entry for the virtual host name of the ASCS/SCS instance by using the following parameters:</span></span>

| <span data-ttu-id="28674-155">Nový název virtuálního hostitele SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="28674-155">New SAP ASCS/SCS virtual host name</span></span> | <span data-ttu-id="28674-156">Přidružené IP adresu</span><span class="sxs-lookup"><span data-stu-id="28674-156">Associated IP address</span></span> |
| --- | --- | --- |
|<span data-ttu-id="28674-157">pr5. sap cl</span><span class="sxs-lookup"><span data-stu-id="28674-157">pr5-sap-cl</span></span> |<span data-ttu-id="28674-158">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="28674-158">10.0.0.50</span></span> |

<span data-ttu-id="28674-159">Novým názvem hostitele a IP adresa se zobrazí ve Správci DNS, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="28674-159">The new host name and IP address are displayed in the DNS Manager, as shown in the following screenshot:</span></span>

![Správce DNS seznamu zvýraznění definované položky DNS pro novou SAP ASC nebo SCS clusteru virtuální název a adresu TCP/IP][sap-ha-guide-figure-6004]

<span data-ttu-id="28674-161">Postup pro vytvoření položky DNS je také popsáno podrobně v hlavní [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="28674-161">The procedure for creating a DNS entry is also described in detail in the main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9.1.1].</span></span>

> [!NOTE]
> <span data-ttu-id="28674-162">Novou IP adresu, která přiřadíte název virtuálního hostitele další instance ASC nebo SCS musí být stejný jako novou IP adresu, který jste přiřadili ke službě Vyrovnávání zatížení SAP Azure.</span><span class="sxs-lookup"><span data-stu-id="28674-162">The new IP address that you assign to the virtual host name of the additional ASCS/SCS instance must be the same as the new IP address that you assigned to the SAP Azure load balancer.</span></span>
>
><span data-ttu-id="28674-163">V našem scénáři je IP adresa 10.0.0.50.</span><span class="sxs-lookup"><span data-stu-id="28674-163">In our scenario, the IP address is 10.0.0.50.</span></span>

### <a name="add-an-ip-address-to-an-existing-azure-internal-load-balancer-by-using-powershell"></a><span data-ttu-id="28674-164">Přidat existující Vyrovnávání zatížení Azure interní IP adresu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="28674-164">Add an IP address to an existing Azure internal load balancer by using PowerShell</span></span>

<span data-ttu-id="28674-165">Pokud chcete vytvořit více než jedna instance SAP ASC nebo SCS ve stejném clusteru služby WSFC, přidat existující Vyrovnávání zatížení Azure interní IP adresu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="28674-165">To create more than one SAP ASCS/SCS instance in the same WSFC cluster, use PowerShell to add an IP address to an existing Azure internal load balancer.</span></span> <span data-ttu-id="28674-166">Každou IP adresu vyžaduje vlastní pravidla Vyrovnávání zatížení, port testu, front-end fond IP adres a fond back-end.</span><span class="sxs-lookup"><span data-stu-id="28674-166">Each IP address requires its own load-balancing rules, probe port, front-end IP pool, and back-end pool.</span></span>

<span data-ttu-id="28674-167">Následující skript přidá novou IP adresu do existující pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="28674-167">The following script adds a new IP address to an existing load balancer.</span></span> <span data-ttu-id="28674-168">Aktualizujte proměnné prostředí PowerShell pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="28674-168">Update the PowerShell variables for your environment.</span></span> <span data-ttu-id="28674-169">Skript se vytvoří všechny potřebné pravidla Vyrovnávání zatížení pro všechny porty SAP ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="28674-169">The script will create all needed load-balancing rules for all SAP ASCS/SCS ports.</span></span>

```powershell

# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get the Azure VNet and subnet
$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzureRmLoadBalancer

# Get new updated configuration
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzureRmLoadBalancer

# Get new updated config
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs to backend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of the '$VMName' VM to the backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for the ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for the port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' to the internal load balancer '$ILBName'!" -ForegroundColor Green

```
<span data-ttu-id="28674-170">Po spuštění skriptu, výsledky se zobrazí na portálu Azure, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="28674-170">After the script has run, the results are displayed in the Azure portal, as shown in the following screenshot:</span></span>

![Nový fond IP front-endu na portálu Azure][sap-ha-guide-figure-6005]

### <a name="add-disks-to-cluster-machines-and-configure-the-sios-cluster-share-disk"></a><span data-ttu-id="28674-172">Přidat disky do clusteru počítačů a konfiguraci disku SIOS clusteru sdílené složky</span><span class="sxs-lookup"><span data-stu-id="28674-172">Add disks to cluster machines, and configure the SIOS cluster share disk</span></span>

<span data-ttu-id="28674-173">Musíte přidat nový disk clusteru sdílené složky pro každou další instanci SAP ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="28674-173">You must add a new cluster share disk for each additional SAP ASCS/SCS instance.</span></span> <span data-ttu-id="28674-174">Pro Windows Server 2012 R2 je disk sdílené složky clusteru služby WSFC aktuálně používán s DataKeeper softwarové řešení.</span><span class="sxs-lookup"><span data-stu-id="28674-174">For Windows Server 2012 R2, the WSFC cluster share disk currently in use is the SIOS DataKeeper software solution.</span></span>

<span data-ttu-id="28674-175">Udělejte toto:</span><span class="sxs-lookup"><span data-stu-id="28674-175">Do the following:</span></span>
1. <span data-ttu-id="28674-176">Přidat další disk nebo disky stejnou velikost (které je třeba rozkládají) na všech uzlech clusteru a jejich formátování.</span><span class="sxs-lookup"><span data-stu-id="28674-176">Add an additional disk or disks of the same size (which you need to stripe) to each of the cluster nodes, and format them.</span></span>
2. <span data-ttu-id="28674-177">Nakonfigurujte s DataKeeper replikace úložiště.</span><span class="sxs-lookup"><span data-stu-id="28674-177">Configure storage replication with SIOS DataKeeper.</span></span>

<span data-ttu-id="28674-178">Tento postup předpokládá, že jste již nainstalovali s DataKeeper u počítačů clusteru služby WSFC.</span><span class="sxs-lookup"><span data-stu-id="28674-178">This procedure assumes that you have already installed SIOS DataKeeper on the WSFC cluster machines.</span></span> <span data-ttu-id="28674-179">Pokud jste ho nainstalovali, musíte teď nakonfigurovat replikace mezi počítači.</span><span class="sxs-lookup"><span data-stu-id="28674-179">If you have installed it, you must now configure replication between the machines.</span></span> <span data-ttu-id="28674-180">Proces je podrobně popsány v hlavní [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide-8.12.3.3].</span><span class="sxs-lookup"><span data-stu-id="28674-180">The process is described in detail in the main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.12.3.3].</span></span>  

![DataKeeper synchronní zrcadlení pro nové SAP ASC nebo SCS sdílet disk][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a><span data-ttu-id="28674-182">Nasazení virtuálních počítačů pro SAP aplikační servery a cluster databázového systému</span><span class="sxs-lookup"><span data-stu-id="28674-182">Deploy VMs for SAP application servers and DBMS cluster</span></span>

<span data-ttu-id="28674-183">K dokončení Příprava infrastruktury pro druhý systému SAP, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="28674-183">To complete the infrastructure preparation for the second SAP system, do the following:</span></span>

1. <span data-ttu-id="28674-184">Nasazení vyhrazených virtuálních počítačích pro SAP aplikační servery a vložte je do své vlastní vyhrazené dostupnosti skupiny.</span><span class="sxs-lookup"><span data-stu-id="28674-184">Deploy dedicated VMs for SAP application servers and put them in their own dedicated availability group.</span></span>
2. <span data-ttu-id="28674-185">Nasazení vyhrazených virtuálních počítačích pro cluster databázového systému a vložte je do své vlastní vyhrazené dostupnosti skupiny.</span><span class="sxs-lookup"><span data-stu-id="28674-185">Deploy dedicated VMs for DBMS cluster and put them in their own dedicated availability group.</span></span>


## <a name="install-the-second-sap-sid2-netweaver-system"></a><span data-ttu-id="28674-186">Instalace druhé systému SAP SID2 NetWeaver</span><span class="sxs-lookup"><span data-stu-id="28674-186">Install the second SAP SID2 NetWeaver system</span></span>

<span data-ttu-id="28674-187">Dokončení procesu instalace druhé systému SAP SID2 je popsaný v hlavní [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide-9].</span><span class="sxs-lookup"><span data-stu-id="28674-187">The complete process of installing a second SAP SID2 system is described in the main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9].</span></span>

<span data-ttu-id="28674-188">Podrobný postup je následující:</span><span class="sxs-lookup"><span data-stu-id="28674-188">The high-level procedure is as follows:</span></span>

1. <span data-ttu-id="28674-189">[Instalace prvního uzlu clusteru SAP][sap-ha-guide-9.1.2].</span><span class="sxs-lookup"><span data-stu-id="28674-189">[Install the SAP first cluster node][sap-ha-guide-9.1.2].</span></span>  
 <span data-ttu-id="28674-190">V tomto kroku instalujete SAP s vysokou dostupností ASC nebo SCS instancí na **uzlu clusteru služby WSFC existující 1**.</span><span class="sxs-lookup"><span data-stu-id="28674-190">In this step, you are installing SAP with a high-availability ASCS/SCS instance on the **EXISTING WSFC cluster node 1**.</span></span>

2. <span data-ttu-id="28674-191">[Upravit profil SAP instance ASC nebo SCS][sap-ha-guide-9.1.3].</span><span class="sxs-lookup"><span data-stu-id="28674-191">[Modify the SAP profile of the ASCS/SCS instance][sap-ha-guide-9.1.3].</span></span>

3. <span data-ttu-id="28674-192">[Nakonfigurujte port testu][sap-ha-guide-9.1.4].</span><span class="sxs-lookup"><span data-stu-id="28674-192">[Configure a probe port][sap-ha-guide-9.1.4].</span></span>  
 <span data-ttu-id="28674-193">V tomto kroku nakonfigurujete prostředek clusteru SAP port testu SAP. SID2 IP pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="28674-193">In this step, you are configuring an SAP cluster resource SAP-SID2-IP probe port by using PowerShell.</span></span> <span data-ttu-id="28674-194">Tuto konfiguraci proveďte v jednom z uzlů clusteru SAP ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="28674-194">Execute this configuration on one of the SAP ASCS/SCS cluster nodes.</span></span>

4. <span data-ttu-id="28674-195">[Nainstalovat instanci databáze] [sap-ha Průvodce-9.2].</span><span class="sxs-lookup"><span data-stu-id="28674-195">[Install the database instance][sap-ha-guide-9.2].</span></span>  
 <span data-ttu-id="28674-196">V tomto kroku instalujete databázového systému na vyhrazeném clusteru služby WSFC.</span><span class="sxs-lookup"><span data-stu-id="28674-196">In this step, you are installing DBMS on a dedicated WSFC cluster.</span></span>

5. <span data-ttu-id="28674-197">[Install druhého uzlu clusteru] [sap-ha Průvodce-9.3].</span><span class="sxs-lookup"><span data-stu-id="28674-197">[Install the second cluster node][sap-ha-guide-9.3].</span></span>  
 <span data-ttu-id="28674-198">V tomto kroku instalujete SAP s vysokou dostupností ASC nebo SCS instancí na stávající uzel clusteru služby WSFC 2.</span><span class="sxs-lookup"><span data-stu-id="28674-198">In this step, you are installing SAP with a high-availability ASCS/SCS instance on the existing WSFC cluster node 2.</span></span>

6. <span data-ttu-id="28674-199">Otevřete porty brány Windows Firewall pro instance SAP ASC nebo SCS a ProbePort.</span><span class="sxs-lookup"><span data-stu-id="28674-199">Open Windows Firewall ports for the SAP ASCS/SCS instance and ProbePort.</span></span>  
 <span data-ttu-id="28674-200">Na obou uzlů clusteru, které se používají pro instance SAP ASC nebo SCS otevíráte všechny porty brány Windows Firewall, které SAP ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="28674-200">On both cluster nodes that are used for SAP ASCS/SCS instances, you are opening all Windows Firewall ports that are used by SAP ASCS/SCS.</span></span> <span data-ttu-id="28674-201">Tyto porty jsou uvedeny v [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide-8.8].</span><span class="sxs-lookup"><span data-stu-id="28674-201">These ports are listed in the [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.8].</span></span>  
 <span data-ttu-id="28674-202">Také otevřete port testu nástroje pro vyrovnávání Azure interní služby load, což je 62350 v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="28674-202">Also open the Azure internal load balancer probe port, which is 62350 in our scenario.</span></span>

7. <span data-ttu-id="28674-203">[Změnit typ spuštění instance služby Windows YBRAT SAP][sap-ha-guide-9.4].</span><span class="sxs-lookup"><span data-stu-id="28674-203">[Change the start type of the SAP ERS Windows service instance][sap-ha-guide-9.4].</span></span>

8. <span data-ttu-id="28674-204">[Instalace serveru primární aplikace SAP] [ sap-ha-guide-9.5] na novém vyhrazeném virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="28674-204">[Install the SAP primary application server][sap-ha-guide-9.5] on the new dedicated VM.</span></span>

9. <span data-ttu-id="28674-205">[Instalace serveru SAP další aplikaci] [ sap-ha-guide-9.6] na novém vyhrazeném virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="28674-205">[Install the SAP additional application server][sap-ha-guide-9.6] on the new dedicated VM.</span></span>

10. <span data-ttu-id="28674-206">[Testovací převzetí služeb při selhání SAP ASC nebo SCS instance a replikace SIOS][sap-ha-guide-10].</span><span class="sxs-lookup"><span data-stu-id="28674-206">[Test the SAP ASCS/SCS instance failover and SIOS replication][sap-ha-guide-10].</span></span>

## <a name="next-steps"></a><span data-ttu-id="28674-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28674-207">Next steps</span></span>

- <span data-ttu-id="28674-208">[Omezení sítě: Azure Resource Manager][networking-limits-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="28674-208">[Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager]</span></span>
- <span data-ttu-id="28674-209">[Nástroj pro vyrovnávání zatížení několika virtuálními IP adresami pro Azure.][load-balancer-multivip-overview]</span><span class="sxs-lookup"><span data-stu-id="28674-209">[Multiple VIPs for Azure Load Balancer][load-balancer-multivip-overview]</span></span>
- <span data-ttu-id="28674-210">[Průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="28674-210">[Guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide]</span></span>
