---
title: "aaaCreate konfigurace aplikace SAP více SID v Azure | Microsoft Docs"
description: "Průvodce toohigh dostupnosti SAP NetWeaver více SID konfiguraci na virtuálních počítačích s Windows"
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
ms.openlocfilehash: e2a4b12928231743c59af86dab72595ad38d364b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a><span data-ttu-id="b49ab-103">Vytvoření konfigurace aplikace SAP NetWeaver více SID</span><span class="sxs-lookup"><span data-stu-id="b49ab-103">Create an SAP NetWeaver multi-SID configuration</span></span>

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

<span data-ttu-id="b49ab-104">V září 2016 společnost Microsoft vydala funkce, kde můžete spravovat pomocí pro vyrovnávání zatížení Azure interní víc virtuálních IP adres.</span><span class="sxs-lookup"><span data-stu-id="b49ab-104">In September 2016, Microsoft released a feature where you can manage multiple virtual IP addresses by using an Azure internal load balancer.</span></span> <span data-ttu-id="b49ab-105">Tato funkce již existuje v hello Azure externím vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="b49ab-105">This functionality already exists in hello Azure external load balancer.</span></span>

<span data-ttu-id="b49ab-106">Pokud máte nasazení SAP, můžete použít interní služby load vyrovnávání toocreate konfiguraci clusteru systému Windows pro SAP ASC nebo SCS, jak je uvedeno v hello [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows] [ sap-ha-guide].</span><span class="sxs-lookup"><span data-stu-id="b49ab-106">If you have an SAP deployment, you can use an internal load balancer toocreate a Windows cluster configuration for SAP ASCS/SCS, as documented in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide].</span></span>

<span data-ttu-id="b49ab-107">Tento článek se zaměřuje na tom, jak toomove z ASC nebo SCS instalace tooan SAP SID více konfigurací jedné nainstalováním dalších SAP ASC nebo SCS Clusterované instance do existujícího clusteru Windows Server Failover Clustering (WSFC).</span><span class="sxs-lookup"><span data-stu-id="b49ab-107">This article focuses on how toomove from a single ASCS/SCS installation tooan SAP multi-SID configuration by installing additional SAP ASCS/SCS clustered instances into an existing Windows Server Failover Clustering (WSFC) cluster.</span></span> <span data-ttu-id="b49ab-108">Po dokončení tohoto procesu jste nakonfigurovali clusteru více SID služby SAP.</span><span class="sxs-lookup"><span data-stu-id="b49ab-108">When this process is completed, you will have configured an SAP multi-SID cluster.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="b49ab-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b49ab-109">Prerequisites</span></span>
<span data-ttu-id="b49ab-110">Jste již nakonfigurovali cluster služby WSFC, který se používá pro jednu instanci SAP ASC nebo SCS, jak je popsáno v hello [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows] [ sap-ha-guide] a jak je znázorněno v tomto diagramu.</span><span class="sxs-lookup"><span data-stu-id="b49ab-110">You have already configured a WSFC cluster that is used for one SAP ASCS/SCS instance, as discussed in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide] and as shown in this diagram.</span></span>

![Instance SAP ASC nebo SCS vysokou dostupnost][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a><span data-ttu-id="b49ab-112">Cílová architektura</span><span class="sxs-lookup"><span data-stu-id="b49ab-112">Target architecture</span></span>

<span data-ttu-id="b49ab-113">Hello cílem je tooinstall více SAP ABAP ASC nebo SAP Java SCS Clusterované instance v hello stejný cluster služby WSFC, jak je znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="b49ab-113">hello goal is tooinstall multiple SAP ABAP ASCS or SAP Java SCS clustered instances in hello same WSFC cluster, as illustrated here:</span></span>

![Více instancí SAP ASC nebo SCS clusteru v Azure][sap-ha-guide-figure-6002]

> [!NOTE]
><span data-ttu-id="b49ab-115">Existuje limit toohello, počet privátní front-end IP adresy pro každý nástroj pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="b49ab-115">There is a limit toohello number of private front-end IPs for each Azure internal load balancer.</span></span>
>
><span data-ttu-id="b49ab-116">maximální počet instancí SAP ASC nebo SCS do jednoho clusteru služby WSFC Hello je rovna toohello maximální počet privátní front-end IP adresy pro každý nástroj pro vyrovnávání zatížení Azure interní.</span><span class="sxs-lookup"><span data-stu-id="b49ab-116">hello maximum number of SAP ASCS/SCS instances in one WSFC cluster is equal toohello maximum number of private front-end IPs for each Azure internal load balancer.</span></span>
>

<span data-ttu-id="b49ab-117">Další informace o omezeních pro vyrovnávání zatížení, najdete v části "privátní front-end IP adresy na nástroj pro vyrovnávání zatížení" v [omezení sítě: Azure Resource Manager][networking-limits-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="b49ab-117">For more information about load-balancer limits, see "Private front end IP per load balancer" in [Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager].</span></span>

<span data-ttu-id="b49ab-118">dokončení na šířku Hello s dvěma systémy SAP vysoké dostupnosti bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="b49ab-118">hello complete landscape with two high-availability SAP systems would look like this:</span></span>

![Nastavení vysoké dostupnosti více SID SAP s dvě systému SAP identifikátorů SID][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> <span data-ttu-id="b49ab-120">Instalační program Hello musí splňovat hello následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="b49ab-120">hello setup must meet hello following conditions:</span></span>
> - <span data-ttu-id="b49ab-121">Hello SAP ASC nebo SCS instance musejí sdílet hello stejný cluster služby WSFC.</span><span class="sxs-lookup"><span data-stu-id="b49ab-121">hello SAP ASCS/SCS instances must share hello same WSFC cluster.</span></span>
> - <span data-ttu-id="b49ab-122">Každý SID databázového systému musí mít svůj vlastní vyhrazený cluster služby WSFC.</span><span class="sxs-lookup"><span data-stu-id="b49ab-122">Each DBMS SID must have its own dedicated WSFC cluster.</span></span>
> - <span data-ttu-id="b49ab-123">SAP aplikační servery, které patří systému SAP tooone SID musí mít vlastní vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="b49ab-123">SAP application servers that belong tooone SAP system SID must have their own dedicated VMs.</span></span>


## <a name="prepare-hello-infrastructure"></a><span data-ttu-id="b49ab-124">Příprava infrastruktury hello</span><span class="sxs-lookup"><span data-stu-id="b49ab-124">Prepare hello infrastructure</span></span>
<span data-ttu-id="b49ab-125">tooprepare infrastrukturu, můžete nainstalovat další instance SAP ASC nebo SCS s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="b49ab-125">tooprepare your infrastructure, you can install an additional SAP ASCS/SCS instance with hello following parameters:</span></span>

| <span data-ttu-id="b49ab-126">Název parametru</span><span class="sxs-lookup"><span data-stu-id="b49ab-126">Parameter name</span></span> | <span data-ttu-id="b49ab-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="b49ab-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="b49ab-128">SAP ASC NEBO SCS SID</span><span class="sxs-lookup"><span data-stu-id="b49ab-128">SAP ASCS/SCS SID</span></span> |<span data-ttu-id="b49ab-129">PR1-lb ASC</span><span class="sxs-lookup"><span data-stu-id="b49ab-129">pr1-lb-ascs</span></span> |
| <span data-ttu-id="b49ab-130">Databázového systému SAP interní nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b49ab-130">SAP DBMS internal load balancer</span></span> | <span data-ttu-id="b49ab-131">PR5</span><span class="sxs-lookup"><span data-stu-id="b49ab-131">PR5</span></span> |
| <span data-ttu-id="b49ab-132">Název virtuálního hostitele SAP</span><span class="sxs-lookup"><span data-stu-id="b49ab-132">SAP virtual host name</span></span> | <span data-ttu-id="b49ab-133">pr5. sap cl</span><span class="sxs-lookup"><span data-stu-id="b49ab-133">pr5-sap-cl</span></span> |
| <span data-ttu-id="b49ab-134">SAP ASC nebo SCS virtuální hostitele IP adresu (IP adresa služby Vyrovnávání zatížení Další Azure)</span><span class="sxs-lookup"><span data-stu-id="b49ab-134">SAP ASCS/SCS virtual host IP address (additional Azure load balancer IP address)</span></span> | <span data-ttu-id="b49ab-135">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="b49ab-135">10.0.0.50</span></span> |
| <span data-ttu-id="b49ab-136">Čísla instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="b49ab-136">SAP ASCS/SCS instance number</span></span> | <span data-ttu-id="b49ab-137">50</span><span class="sxs-lookup"><span data-stu-id="b49ab-137">50</span></span> |
| <span data-ttu-id="b49ab-138">Port testu ILB pro další instance SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="b49ab-138">ILB probe port for additional SAP ASCS/SCS instance</span></span> | <span data-ttu-id="b49ab-139">62350</span><span class="sxs-lookup"><span data-stu-id="b49ab-139">62350</span></span> |

> [!NOTE]
> <span data-ttu-id="b49ab-140">Každou IP adresu pro SAP ASC nebo SCS instance clusteru, vyžaduje port jedinečný testu.</span><span class="sxs-lookup"><span data-stu-id="b49ab-140">For SAP ASCS/SCS cluster instances, each IP address requires a unique probe port.</span></span> <span data-ttu-id="b49ab-141">Například pokud jednu IP adresu na Azure interní nástroj používá port testu 62300, žádné jiné IP adresy na tento nástroj pro vyrovnávání zatížení můžete použít port testu 62300.</span><span class="sxs-lookup"><span data-stu-id="b49ab-141">For example, if one IP address on an Azure internal load balancer uses probe port 62300, no other IP address on that load balancer can use probe port 62300.</span></span>
>
><span data-ttu-id="b49ab-142">Pro naše účely protože port testu 62300 je rezervovaná, se používá port testu 62350.</span><span class="sxs-lookup"><span data-stu-id="b49ab-142">For our purposes, because probe port 62300 is already reserved, we are using probe port 62350.</span></span>

<span data-ttu-id="b49ab-143">Můžete nainstalovat další instance SAP ASC nebo SCS v hello existující cluster služby WSFC s dvěma uzly:</span><span class="sxs-lookup"><span data-stu-id="b49ab-143">You can install additional SAP ASCS/SCS instances in hello existing WSFC cluster with two nodes:</span></span>

| <span data-ttu-id="b49ab-144">Role virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b49ab-144">Virtual machine role</span></span> | <span data-ttu-id="b49ab-145">Název hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b49ab-145">Virtual machine host name</span></span> | <span data-ttu-id="b49ab-146">Statická IP adresa</span><span class="sxs-lookup"><span data-stu-id="b49ab-146">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b49ab-147">1. uzel clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="b49ab-147">1st cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="b49ab-148">PR1-ASC-0</span><span class="sxs-lookup"><span data-stu-id="b49ab-148">pr1-ascs-0</span></span> |<span data-ttu-id="b49ab-149">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="b49ab-149">10.0.0.10</span></span> |
| <span data-ttu-id="b49ab-150">2. uzel clusteru pro instanci ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="b49ab-150">2nd cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="b49ab-151">PR1-ASC-1</span><span class="sxs-lookup"><span data-stu-id="b49ab-151">pr1-ascs-1</span></span> |<span data-ttu-id="b49ab-152">10.0.0.9</span><span class="sxs-lookup"><span data-stu-id="b49ab-152">10.0.0.9</span></span> |

### <a name="create-a-virtual-host-name-for-hello-clustered-sap-ascsscs-instance-on-hello-dns-server"></a><span data-ttu-id="b49ab-153">Na serveru DNS hello vytvořte název virtuálního hostitele pro instance SAP ASC nebo SCS hello v clusteru</span><span class="sxs-lookup"><span data-stu-id="b49ab-153">Create a virtual host name for hello clustered SAP ASCS/SCS instance on hello DNS server</span></span>

<span data-ttu-id="b49ab-154">Položku DNS pro název virtuálního hostitele hello hello ASC nebo SCS instance můžete vytvořit pomocí hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="b49ab-154">You can create a DNS entry for hello virtual host name of hello ASCS/SCS instance by using hello following parameters:</span></span>

| <span data-ttu-id="b49ab-155">Nový název virtuálního hostitele SAP ASC nebo SCS</span><span class="sxs-lookup"><span data-stu-id="b49ab-155">New SAP ASCS/SCS virtual host name</span></span> | <span data-ttu-id="b49ab-156">Přidružené IP adresu</span><span class="sxs-lookup"><span data-stu-id="b49ab-156">Associated IP address</span></span> |
| --- | --- | --- |
|<span data-ttu-id="b49ab-157">pr5. sap cl</span><span class="sxs-lookup"><span data-stu-id="b49ab-157">pr5-sap-cl</span></span> |<span data-ttu-id="b49ab-158">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="b49ab-158">10.0.0.50</span></span> |

<span data-ttu-id="b49ab-159">Hello novým názvem hostitele a IP adresa se zobrazí v hello Správce DNS, jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="b49ab-159">hello new host name and IP address are displayed in hello DNS Manager, as shown in hello following screenshot:</span></span>

![Správce DNS seznamu zvýraznění hello definované položku DNS pro hello nové SAP ASC nebo SCS virtuální název clusteru a adresa TCP/IP][sap-ha-guide-figure-6004]

<span data-ttu-id="b49ab-161">Hello postup pro vytvoření položky DNS je taky podrobně popsaná v v hello hlavní [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="b49ab-161">hello procedure for creating a DNS entry is also described in detail in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9.1.1].</span></span>

> [!NOTE]
> <span data-ttu-id="b49ab-162">Hello novou IP adresu přiřadit název virtuálního hostitele toohello hello další ASC nebo SCS instance musí být hello stejné jako hello novou IP adresu, že jste přiřadili nástroj pro vyrovnávání zatížení toohello SAP Azure.</span><span class="sxs-lookup"><span data-stu-id="b49ab-162">hello new IP address that you assign toohello virtual host name of hello additional ASCS/SCS instance must be hello same as hello new IP address that you assigned toohello SAP Azure load balancer.</span></span>
>
><span data-ttu-id="b49ab-163">V našem scénáři hello IP adresa je 10.0.0.50.</span><span class="sxs-lookup"><span data-stu-id="b49ab-163">In our scenario, hello IP address is 10.0.0.50.</span></span>

### <a name="add-an-ip-address-tooan-existing-azure-internal-load-balancer-by-using-powershell"></a><span data-ttu-id="b49ab-164">Přidat IP adresu tooan existující interní pro vyrovnávání zatížení Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b49ab-164">Add an IP address tooan existing Azure internal load balancer by using PowerShell</span></span>

<span data-ttu-id="b49ab-165">toocreate více než jedna instance SAP ASC nebo SCS v hello stejné služby WSFC clusteru, použijte IP adresu tooan stávající nástroje pro vyrovnávání zatížení Azure interní tooadd prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b49ab-165">toocreate more than one SAP ASCS/SCS instance in hello same WSFC cluster, use PowerShell tooadd an IP address tooan existing Azure internal load balancer.</span></span> <span data-ttu-id="b49ab-166">Každou IP adresu vyžaduje vlastní pravidla Vyrovnávání zatížení, port testu, front-end fond IP adres a fond back-end.</span><span class="sxs-lookup"><span data-stu-id="b49ab-166">Each IP address requires its own load-balancing rules, probe port, front-end IP pool, and back-end pool.</span></span>

<span data-ttu-id="b49ab-167">Hello následující skript přidá novou IP adresu tooan existující Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="b49ab-167">hello following script adds a new IP address tooan existing load balancer.</span></span> <span data-ttu-id="b49ab-168">Aktualizujte hello proměnné prostředí PowerShell pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="b49ab-168">Update hello PowerShell variables for your environment.</span></span> <span data-ttu-id="b49ab-169">skript Hello vytvoří všechny potřebné pravidla Vyrovnávání zatížení pro všechny porty SAP ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="b49ab-169">hello script will create all needed load-balancing rules for all SAP ASCS/SCS ports.</span></span>

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

# Get hello Azure VNet and subnet
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

# Assign VM NICs toobackend pool
$BEPool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of hello '$VMName' VM toohello backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        #start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for hello ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for hello port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzureRmLoadBalancer

Write-Host "Succesfully added new IP '$ILBIP' toohello internal load balancer '$ILBName'!" -ForegroundColor Green

```
<span data-ttu-id="b49ab-170">Po spuštění skriptu hello, hello hello výsledky jsou zobrazeny v portálu Azure, jak ukazuje následující snímek obrazovky hello:</span><span class="sxs-lookup"><span data-stu-id="b49ab-170">After hello script has run, hello results are displayed in hello Azure portal, as shown in hello following screenshot:</span></span>

![Nový fond IP front-endu v hello portálu Azure][sap-ha-guide-figure-6005]

### <a name="add-disks-toocluster-machines-and-configure-hello-sios-cluster-share-disk"></a><span data-ttu-id="b49ab-172">Přidat disky toocluster počítače a nakonfigurovat disk sdílené složky clusteru SIOS hello</span><span class="sxs-lookup"><span data-stu-id="b49ab-172">Add disks toocluster machines, and configure hello SIOS cluster share disk</span></span>

<span data-ttu-id="b49ab-173">Musíte přidat nový disk clusteru sdílené složky pro každou další instanci SAP ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="b49ab-173">You must add a new cluster share disk for each additional SAP ASCS/SCS instance.</span></span> <span data-ttu-id="b49ab-174">Pro Windows Server 2012 R2 je disk sdílené složky clusteru služby WSFC hello aktuálně používán hello s DataKeeper softwarové řešení.</span><span class="sxs-lookup"><span data-stu-id="b49ab-174">For Windows Server 2012 R2, hello WSFC cluster share disk currently in use is hello SIOS DataKeeper software solution.</span></span>

<span data-ttu-id="b49ab-175">Hello následující:</span><span class="sxs-lookup"><span data-stu-id="b49ab-175">Do hello following:</span></span>
1. <span data-ttu-id="b49ab-176">Přidat další disk nebo disky hello stejnou velikost (které je třeba toostripe) tooeach hello uzly clusteru a jejich formátování.</span><span class="sxs-lookup"><span data-stu-id="b49ab-176">Add an additional disk or disks of hello same size (which you need toostripe) tooeach of hello cluster nodes, and format them.</span></span>
2. <span data-ttu-id="b49ab-177">Nakonfigurujte s DataKeeper replikace úložiště.</span><span class="sxs-lookup"><span data-stu-id="b49ab-177">Configure storage replication with SIOS DataKeeper.</span></span>

<span data-ttu-id="b49ab-178">Tento postup předpokládá, že jste již nainstalovali s DataKeeper u počítačů clusteru služby WSFC hello.</span><span class="sxs-lookup"><span data-stu-id="b49ab-178">This procedure assumes that you have already installed SIOS DataKeeper on hello WSFC cluster machines.</span></span> <span data-ttu-id="b49ab-179">Pokud jste ho nainstalovali, musíte teď nakonfigurovat replikace mezi počítači hello.</span><span class="sxs-lookup"><span data-stu-id="b49ab-179">If you have installed it, you must now configure replication between hello machines.</span></span> <span data-ttu-id="b49ab-180">proces Hello je podrobně popsaná v hello hlavní [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide-8.12.3.3].</span><span class="sxs-lookup"><span data-stu-id="b49ab-180">hello process is described in detail in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.12.3.3].</span></span>  

![DataKeeper synchronní zrcadlení pro hello nový disk SAP ASC nebo SCS sdílené složky][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a><span data-ttu-id="b49ab-182">Nasazení virtuálních počítačů pro SAP aplikační servery a cluster databázového systému</span><span class="sxs-lookup"><span data-stu-id="b49ab-182">Deploy VMs for SAP application servers and DBMS cluster</span></span>

<span data-ttu-id="b49ab-183">Příprava infrastruktury hello toocomplete hello druhý systému SAP, hello následující:</span><span class="sxs-lookup"><span data-stu-id="b49ab-183">toocomplete hello infrastructure preparation for hello second SAP system, do hello following:</span></span>

1. <span data-ttu-id="b49ab-184">Nasazení vyhrazených virtuálních počítačích pro SAP aplikační servery a vložte je do své vlastní vyhrazené dostupnosti skupiny.</span><span class="sxs-lookup"><span data-stu-id="b49ab-184">Deploy dedicated VMs for SAP application servers and put them in their own dedicated availability group.</span></span>
2. <span data-ttu-id="b49ab-185">Nasazení vyhrazených virtuálních počítačích pro cluster databázového systému a vložte je do své vlastní vyhrazené dostupnosti skupiny.</span><span class="sxs-lookup"><span data-stu-id="b49ab-185">Deploy dedicated VMs for DBMS cluster and put them in their own dedicated availability group.</span></span>


## <a name="install-hello-second-sap-sid2-netweaver-system"></a><span data-ttu-id="b49ab-186">Instalace systému SAP SID2 NetWeaver druhý hello</span><span class="sxs-lookup"><span data-stu-id="b49ab-186">Install hello second SAP SID2 NetWeaver system</span></span>

<span data-ttu-id="b49ab-187">Hello dokončení procesu instalace druhé systému SAP SID2 je popsaná v hello hlavní [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide-9].</span><span class="sxs-lookup"><span data-stu-id="b49ab-187">hello complete process of installing a second SAP SID2 system is described in hello main [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-9].</span></span>

<span data-ttu-id="b49ab-188">Hello podrobný postup je následující:</span><span class="sxs-lookup"><span data-stu-id="b49ab-188">hello high-level procedure is as follows:</span></span>

1. <span data-ttu-id="b49ab-189">[Instalace prvního uzlu clusteru hello SAP][sap-ha-guide-9.1.2].</span><span class="sxs-lookup"><span data-stu-id="b49ab-189">[Install hello SAP first cluster node][sap-ha-guide-9.1.2].</span></span>  
 <span data-ttu-id="b49ab-190">V tomto kroku instalujete SAP s vysokou dostupností ASC nebo SCS instancí na hello **uzlu clusteru služby WSFC existující 1**.</span><span class="sxs-lookup"><span data-stu-id="b49ab-190">In this step, you are installing SAP with a high-availability ASCS/SCS instance on hello **EXISTING WSFC cluster node 1**.</span></span>

2. <span data-ttu-id="b49ab-191">[Upravit profil SAP hello hello ASC nebo SCS instance][sap-ha-guide-9.1.3].</span><span class="sxs-lookup"><span data-stu-id="b49ab-191">[Modify hello SAP profile of hello ASCS/SCS instance][sap-ha-guide-9.1.3].</span></span>

3. <span data-ttu-id="b49ab-192">[Nakonfigurujte port testu][sap-ha-guide-9.1.4].</span><span class="sxs-lookup"><span data-stu-id="b49ab-192">[Configure a probe port][sap-ha-guide-9.1.4].</span></span>  
 <span data-ttu-id="b49ab-193">V tomto kroku nakonfigurujete prostředek clusteru SAP port testu SAP. SID2 IP pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b49ab-193">In this step, you are configuring an SAP cluster resource SAP-SID2-IP probe port by using PowerShell.</span></span> <span data-ttu-id="b49ab-194">Tuto konfiguraci proveďte jeden z uzlů clusteru SAP ASC nebo SCS hello.</span><span class="sxs-lookup"><span data-stu-id="b49ab-194">Execute this configuration on one of hello SAP ASCS/SCS cluster nodes.</span></span>

4. <span data-ttu-id="b49ab-195">[Nainstalovat instanci databáze hello] [sap-ha Průvodce-9.2].</span><span class="sxs-lookup"><span data-stu-id="b49ab-195">[Install hello database instance][sap-ha-guide-9.2].</span></span>  
 <span data-ttu-id="b49ab-196">V tomto kroku instalujete databázového systému na vyhrazeném clusteru služby WSFC.</span><span class="sxs-lookup"><span data-stu-id="b49ab-196">In this step, you are installing DBMS on a dedicated WSFC cluster.</span></span>

5. <span data-ttu-id="b49ab-197">[Install druhého uzlu clusteru hello] [sap-ha Průvodce-9.3].</span><span class="sxs-lookup"><span data-stu-id="b49ab-197">[Install hello second cluster node][sap-ha-guide-9.3].</span></span>  
 <span data-ttu-id="b49ab-198">V tomto kroku instalujete SAP s vysokou dostupností ASC nebo SCS instancí na hello WSFC uzlu stávajícího clusteru 2.</span><span class="sxs-lookup"><span data-stu-id="b49ab-198">In this step, you are installing SAP with a high-availability ASCS/SCS instance on hello existing WSFC cluster node 2.</span></span>

6. <span data-ttu-id="b49ab-199">Otevřete porty brány Windows Firewall pro instance SAP ASC nebo SCS hello a ProbePort.</span><span class="sxs-lookup"><span data-stu-id="b49ab-199">Open Windows Firewall ports for hello SAP ASCS/SCS instance and ProbePort.</span></span>  
 <span data-ttu-id="b49ab-200">Na obou uzlů clusteru, které se používají pro instance SAP ASC nebo SCS otevíráte všechny porty brány Windows Firewall, které SAP ASC nebo SCS.</span><span class="sxs-lookup"><span data-stu-id="b49ab-200">On both cluster nodes that are used for SAP ASCS/SCS instances, you are opening all Windows Firewall ports that are used by SAP ASCS/SCS.</span></span> <span data-ttu-id="b49ab-201">Tyto porty jsou uvedeny v hello [průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide-8.8].</span><span class="sxs-lookup"><span data-stu-id="b49ab-201">These ports are listed in hello [guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide-8.8].</span></span>  
 <span data-ttu-id="b49ab-202">Také otevřete hello zatížení Azure interní nástroj pro vyrovnávání testu port, který je 62350 v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="b49ab-202">Also open hello Azure internal load balancer probe port, which is 62350 in our scenario.</span></span>

7. <span data-ttu-id="b49ab-203">[Změňte typ spouštění hello instance služby SAP YBRAT Windows hello][sap-ha-guide-9.4].</span><span class="sxs-lookup"><span data-stu-id="b49ab-203">[Change hello start type of hello SAP ERS Windows service instance][sap-ha-guide-9.4].</span></span>

8. <span data-ttu-id="b49ab-204">[Instalace serveru primární aplikace SAP hello] [ sap-ha-guide-9.5] na hello nové vyhrazený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b49ab-204">[Install hello SAP primary application server][sap-ha-guide-9.5] on hello new dedicated VM.</span></span>

9. <span data-ttu-id="b49ab-205">[Instalace serveru další aplikace SAP hello] [ sap-ha-guide-9.6] na hello nové vyhrazený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b49ab-205">[Install hello SAP additional application server][sap-ha-guide-9.6] on hello new dedicated VM.</span></span>

10. <span data-ttu-id="b49ab-206">[Testovací převzetí služeb při selhání hello SAP ASC nebo SCS instance a replikace SIOS][sap-ha-guide-10].</span><span class="sxs-lookup"><span data-stu-id="b49ab-206">[Test hello SAP ASCS/SCS instance failover and SIOS replication][sap-ha-guide-10].</span></span>

## <a name="next-steps"></a><span data-ttu-id="b49ab-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b49ab-207">Next steps</span></span>

- <span data-ttu-id="b49ab-208">[Omezení sítě: Azure Resource Manager][networking-limits-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="b49ab-208">[Networking limits: Azure Resource Manager][networking-limits-azure-resource-manager]</span></span>
- <span data-ttu-id="b49ab-209">[Nástroj pro vyrovnávání zatížení několika virtuálními IP adresami pro Azure.][load-balancer-multivip-overview]</span><span class="sxs-lookup"><span data-stu-id="b49ab-209">[Multiple VIPs for Azure Load Balancer][load-balancer-multivip-overview]</span></span>
- <span data-ttu-id="b49ab-210">[Průvodci pro vysokou dostupnost SAP NetWeaver na virtuálních počítačích Windows][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="b49ab-210">[Guide for high-availability SAP NetWeaver on Windows VMs][sap-ha-guide]</span></span>
