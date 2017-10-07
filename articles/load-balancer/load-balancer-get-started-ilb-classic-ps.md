---
title: "aaaCreate interní Azure pro vyrovnávání zátěže - PowerShell classic | Microsoft Docs"
description: "Zjistěte, jak toocreate na interní nástroj pro vyrovnávání pomocí prostředí PowerShell v modelu nasazení classic hello zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 3be93168-3787-45a5-a194-9124fe386493
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 382db80c42ffab09905513019b72e85a4f9dfeff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a><span data-ttu-id="8acc5-103">Začínáme vytvářet interní nástroj pro vyrovnávání zatížení (Classic) pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8acc5-103">Get started creating an internal load balancer (classic) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8acc5-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8acc5-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="8acc5-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8acc5-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="8acc5-106">Cloudové služby</span><span class="sxs-lookup"><span data-stu-id="8acc5-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="8acc5-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="8acc5-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="8acc5-108">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="8acc5-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="8acc5-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="8acc5-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="8acc5-110">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="8acc5-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="8acc5-111">Vytvoření sady interního nástroje pro vyrovnávání zatížení pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8acc5-111">Create an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="8acc5-112">toocreate interní nástroj nastavit a hello servery, které se odesílají tooit jejich provoz, máte následující toodo hello:</span><span class="sxs-lookup"><span data-stu-id="8acc5-112">toocreate an internal load balancer set and hello servers that will send their traffic tooit, you have toodo hello following:</span></span>

1. <span data-ttu-id="8acc5-113">Vytvoření instance interní Vyrovnávání zatížení, bude koncový bod hello příchozí provoz toobe vyrovnáváno zatížení napříč servery hello sady Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="8acc5-113">Create an instance of Internal Load Balancing that will be hello endpoint of incoming traffic toobe load balanced across hello servers of a load-balanced set.</span></span>
2. <span data-ttu-id="8acc5-114">Přidáte koncové body odpovídající toohello virtuálních počítačů, které bude moci přijmout příchozí provoz hello.</span><span class="sxs-lookup"><span data-stu-id="8acc5-114">Add endpoints corresponding toohello virtual machines that will be receiving hello incoming traffic.</span></span>
3. <span data-ttu-id="8acc5-115">Konfiguraci hello serverů, které se budou odesílat, že hello provoz toobe s vyrovnáváním zatížení se toosend jejich provoz toohello virtuální adresa IP (VIP) instance hello interní Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8acc5-115">Configure hello servers that will be sending hello traffic toobe load balanced toosend their traffic toohello virtual IP (VIP) address of hello Internal Load Balancing instance.</span></span>

### <a name="step-1-create-an-internal-load-balancing-instance"></a><span data-ttu-id="8acc5-116">Krok 1: Vytvoření instance interního vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8acc5-116">Step 1: Create an Internal Load Balancing instance</span></span>

<span data-ttu-id="8acc5-117">Pro stávající cloudovou službu nebo cloudové služby nasadit v rámci regionální virtuální síť můžete vytvořit instanci interní Vyrovnávání zatížení s hello následující příkazy prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8acc5-117">For an existing cloud service or a cloud service deployed under a regional virtual network, you can create an Internal Load Balancing instance with hello following Windows PowerShell commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
$ilb="<Name of your ILB instance>"
$subnet="<Name of hello subnet within your virtual network>"
$IP="<hello IPv4 address toouse on hello subnet-optional>"

Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP
```

<span data-ttu-id="8acc5-118">Všimněte si, že toto využití hello [přidat AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) rutiny prostředí Windows PowerShell používá sada parametrů DefaultProbe hello.</span><span class="sxs-lookup"><span data-stu-id="8acc5-118">Note that this use of hello [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell cmdlet uses hello DefaultProbe parameter set.</span></span> <span data-ttu-id="8acc5-119">Více informací o dalších sadách parametrů najdete v dokumentaci k rutině [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span><span class="sxs-lookup"><span data-stu-id="8acc5-119">For more information on additional parameter sets, see [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).</span></span>

### <a name="step-2-add-endpoints-toohello-internal-load-balancing-instance"></a><span data-ttu-id="8acc5-120">Krok 2: Přidáte instanci interní Vyrovnávání zatížení toohello koncové body</span><span class="sxs-lookup"><span data-stu-id="8acc5-120">Step 2: Add endpoints toohello Internal Load Balancing instance</span></span>

<span data-ttu-id="8acc5-121">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="8acc5-121">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
$lbsetname="lbset"
$prot="tcp"
$locport=1433
$pubport=1433
$ilb="ilbset"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

### <a name="step-3-configure-your-servers-toosend-their-traffic-toohello-new-internal-load-balancing-endpoint"></a><span data-ttu-id="8acc5-122">Krok 3: Konfigurace vaše servery toosend jejich provoz toohello nový interní Vyrovnávání zatížení koncového bodu</span><span class="sxs-lookup"><span data-stu-id="8acc5-122">Step 3: Configure your servers toosend their traffic toohello new Internal Load Balancing endpoint</span></span>

<span data-ttu-id="8acc5-123">Nakonfigurujete mít příliš hello servery, jejichž provoz je probíhající toobe skupinu s vyrovnáváním zatížení toouse hello novou IP adresu (hello VIP) hello instanci interní Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8acc5-123">You have too configure hello servers whose traffic is going toobe load balanced toouse hello new IP address (hello VIP) of hello Internal Load Balancing instance.</span></span> <span data-ttu-id="8acc5-124">Toto je adresa hello, na které hello interní Vyrovnávání zatížení naslouchá instance.</span><span class="sxs-lookup"><span data-stu-id="8acc5-124">This is hello address on which hello Internal Load Balancing instance is listening.</span></span> <span data-ttu-id="8acc5-125">Ve většině případů potřebujete toojust přidat nebo upravit záznam DNS pro hello VIP instance hello interní Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8acc5-125">In most cases, you need toojust add or modify a DNS record for hello VIP of hello Internal Load Balancing instance.</span></span>

<span data-ttu-id="8acc5-126">Pokud jste zadali IP adresu hello během vytváření hello instance hello interní Vyrovnávání zatížení, už máte hello VIP.</span><span class="sxs-lookup"><span data-stu-id="8acc5-126">If you specified hello IP address during hello creation of hello Internal Load Balancing instance, you already have hello VIP.</span></span> <span data-ttu-id="8acc5-127">Jinak uvidíte hello VIP z hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8acc5-127">Otherwise, you can see hello VIP from hello following commands:</span></span>

```powershell
$svc="<Cloud Service Name>"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="8acc5-128">toouse tyto příkazy, vyplňte hodnoty hello a odebrat hello < a >.</span><span class="sxs-lookup"><span data-stu-id="8acc5-128">toouse these commands, fill in hello values and remove hello < and >.</span></span> <span data-ttu-id="8acc5-129">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="8acc5-129">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer
```

<span data-ttu-id="8acc5-130">Z hello zobrazení hello příkaz Get-AzureInternalLoadBalancer poznamenejte si hello IP adresu a zajistěte, aby hello potřebné změny tooyour servery nebo tooensure záznamy DNS, který získá data odeslaná toohello VIP.</span><span class="sxs-lookup"><span data-stu-id="8acc5-130">From hello display of hello Get-AzureInternalLoadBalancer command, note hello IP address and make hello necessary changes tooyour servers or DNS records tooensure that traffic gets sent toohello VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="8acc5-131">Platforma Microsoft Azure Hello používá statické veřejně směrovatelné IPv4 adresu pro různé scénáře pro správu.</span><span class="sxs-lookup"><span data-stu-id="8acc5-131">hello Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="8acc5-132">Hello IP adresa je 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="8acc5-132">hello IP address is 168.63.129.16.</span></span> <span data-ttu-id="8acc5-133">Tuto IP adresu by neměla blokovat žádná brána firewall, protože by to mohlo způsobit neočekávané chování.</span><span class="sxs-lookup"><span data-stu-id="8acc5-133">This IP address should not be blocked by any firewalls, because it can cause unexpected behavior.</span></span>
> <span data-ttu-id="8acc5-134">S ohledem tooAzure interní Vyrovnávání zatížení tato IP adresa je používán monitorování sondy z hello zatížení vyrovnávání toodetermine hello stav pro virtuální počítače v skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="8acc5-134">With respect tooAzure Internal Load Balancing, this IP address is used by monitoring probes from hello load balancer toodetermine hello health state for virtual machines in a load balanced set.</span></span> <span data-ttu-id="8acc5-135">Pokud skupina zabezpečení sítě je použité toorestrict provoz tooAzure virtuálních počítačů v sadu interně Vyrovnávání zatížení sítě nebo je použité tooa podsíť virtuální sítě, ujistěte se, zda pravidla zabezpečení sítě je přidána tooallow provoz z 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="8acc5-135">If a Network Security Group is used toorestrict traffic tooAzure virtual machines in an internally load-balanced set or is applied tooa Virtual Network Subnet, ensure that a Network Security Rule is added tooallow traffic from 168.63.129.16.</span></span>

## <a name="example-of-internal-load-balancing"></a><span data-ttu-id="8acc5-136">Příklad interního vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8acc5-136">Example of internal load balancing</span></span>

<span data-ttu-id="8acc5-137">toostep prostřednictvím hello koncoví tooend proces vytváření sadu Vyrovnávání zatížení sítě pro dvě konfigurace příklad zobrazí hello následující části.</span><span class="sxs-lookup"><span data-stu-id="8acc5-137">toostep you through hello end-tooend process of creating a load-balanced set for two example configurations, see hello following sections.</span></span>

### <a name="an-internet-facing-multi-tier-application"></a><span data-ttu-id="8acc5-138">Internetová, vícevrstvá aplikace</span><span class="sxs-lookup"><span data-stu-id="8acc5-138">An Internet facing, multi-tier application</span></span>

<span data-ttu-id="8acc5-139">Chcete tooprovide služby Vyrovnávání zatížení databáze pro sadu internetové webové servery.</span><span class="sxs-lookup"><span data-stu-id="8acc5-139">You want tooprovide a load balanced database service for  a set of Internet-facing web servers.</span></span> <span data-ttu-id="8acc5-140">Hostitelem obou sad serverů je jedna cloudová služba Azure.</span><span class="sxs-lookup"><span data-stu-id="8acc5-140">Both sets of servers are hosted in a single Azure cloud service.</span></span> <span data-ttu-id="8acc5-141">Webový server provoz tooTCP port 1433 musí být distribuována mezi dvěma virtuálními počítači v hello databázové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="8acc5-141">Web server traffic tooTCP port 1433 must be distributed among two virtual machines in hello database tier.</span></span> <span data-ttu-id="8acc5-142">Obrázek 1 zobrazuje konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="8acc5-142">Figure 1 shows hello configuration.</span></span>

![Interní sada Vyrovnávání zatížení sítě pro hello databázové vrstvy](./media/load-balancer-internal-getstarted/IC736321.png)

<span data-ttu-id="8acc5-144">Konfigurace Hello se skládá z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="8acc5-144">hello configuration consists of hello following:</span></span>

* <span data-ttu-id="8acc5-145">Hello stávající cloudovou službu hostování hello virtuálních počítačů je s názvem mytestcloud.</span><span class="sxs-lookup"><span data-stu-id="8acc5-145">hello existing cloud service hosting hello virtual machines is named mytestcloud.</span></span>
* <span data-ttu-id="8acc5-146">Hello dvě existující databázové servery jsou pojmenované DB1 DB2.</span><span class="sxs-lookup"><span data-stu-id="8acc5-146">hello two existing database servers are named DB1, DB2.</span></span>
* <span data-ttu-id="8acc5-147">Webové servery v hello webová vrstva připojit servery databáze toohello hello databázové vrstvy pomocí hello privátní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8acc5-147">Web servers in hello web tier connect toohello database servers in hello database tier by using hello private IP address.</span></span> <span data-ttu-id="8acc5-148">Další možností je toouse vlastní DNS pro virtuální síť hello a ruční registraci záznamu A pro sadu Nástroje pro vyrovnávání zatížení interní hello.</span><span class="sxs-lookup"><span data-stu-id="8acc5-148">Another option is toouse your own DNS for hello virtual network and manually register an A record for hello internal load balancer set.</span></span>

<span data-ttu-id="8acc5-149">Hello následující příkazy nakonfigurujte novou instanci interní Vyrovnávání zatížení s názvem **ILBset** a přidat koncové body toohello virtuálních počítačů odpovídající toohello dva databázové servery:</span><span class="sxs-lookup"><span data-stu-id="8acc5-149">hello following commands configure a new Internal Load Balancing instance named **ILBset** and add endpoints toohello virtual machines corresponding toohello two database servers:</span></span>

```powershell
$svc="mytestcloud"
$ilb="ilbset"
Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
$prot="tcp"
$locport=1433
$pubport=1433
$epname="TCP-1433-1433"
$lbsetname="lbset"
$vmname="DB1"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

$epname="TCP-1433-1433-2"
$vmname="DB2"
Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM
```

## <a name="remove-an-internal-load-balancing-configuration"></a><span data-ttu-id="8acc5-150">Odebrání konfigurace interního vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8acc5-150">Remove an Internal Load Balancing configuration</span></span>

<span data-ttu-id="8acc5-151">tooremove virtuálního počítače jako koncový bod z instance nástroje pro vyrovnávání zatížení interní hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8acc5-151">tooremove a virtual machine as an endpoint from an internal load balancer instance, use hello following commands:</span></span>

```powershell
$svc="<Cloud service name>"
$vmname="<Name of hello VM>"
$epname="<Name of hello endpoint>"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="8acc5-152">toouse tyto příkazy vyplnit hello hodnoty, odstranění hello < a >.</span><span class="sxs-lookup"><span data-stu-id="8acc5-152">toouse these commands, fill in hello values, removing hello < and >.</span></span>

<span data-ttu-id="8acc5-153">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="8acc5-153">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
$vmname="DB1"
$epname="TCP-1433-1433"
Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM
```

<span data-ttu-id="8acc5-154">tooremove instance nástroje pro vyrovnávání zatížení interní z cloudové služby, hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8acc5-154">tooremove an internal load balancer instance from a cloud service, use hello following commands:</span></span>

```powershell
$svc="<Cloud service name>"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

<span data-ttu-id="8acc5-155">Zadejte hodnotu hello toouse tyto příkazy, a odeberte hello < a >.</span><span class="sxs-lookup"><span data-stu-id="8acc5-155">toouse these commands, fill in hello value and remove hello < and >.</span></span>

<span data-ttu-id="8acc5-156">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="8acc5-156">Here is an example:</span></span>

```powershell
$svc="mytestcloud"
Remove-AzureInternalLoadBalancer -ServiceName $svc
```

## <a name="additional-information-about-internal-load-balancer-cmdlets"></a><span data-ttu-id="8acc5-157">Další informace o rutinách interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8acc5-157">Additional information about internal load balancer cmdlets</span></span>

<span data-ttu-id="8acc5-158">tooobtain Další informace o rutinách interní Vyrovnávání zatížení, spusťte následující příkazy příkazového řádku Windows Powershellu hello:</span><span class="sxs-lookup"><span data-stu-id="8acc5-158">tooobtain additional information about Internal Load Balancing cmdlets, run hello following commands at a Windows PowerShell prompt:</span></span>

```powershell
Get-Help New-AzureInternalLoadBalancerConfig -full
Get-Help Add-AzureInternalLoadBalancer -full
Get-Help Get-AzureInternalLoadbalancer -full
Get-Help Remove-AzureInternalLoadBalancer -full
```

## <a name="next-steps"></a><span data-ttu-id="8acc5-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8acc5-159">Next steps</span></span>

[<span data-ttu-id="8acc5-160">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou</span><span class="sxs-lookup"><span data-stu-id="8acc5-160">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="8acc5-161">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8acc5-161">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

