---
title: "aaaCreate Azure internetového vyrovnávání zátěže – prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate internetové službu Vyrovnávání zatížení ve službě Správce prostředků pomocí prostředí PowerShell"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e4e0e5271bc83c23fc62c0910e784c57d2b30065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="11bea-103"><a name="get-started"></a>Vytvoření internetového nástroje pro vyrovnávání zatížení v Resource Manageru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="11bea-103"><a name="get-started"></a>Creating an Internet-facing load balancer in Resource Manager by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="11bea-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="11bea-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="11bea-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="11bea-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="11bea-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="11bea-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="11bea-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="11bea-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="11bea-108">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="11bea-109">Můžete také [zjistěte, jak toocreate internetové nástroj pro vyrovnávání zatížení pomocí modelu nasazení classic hello](load-balancer-get-started-internet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="11bea-109">You can also [learn how toocreate an Internet-facing load balancer by using hello classic deployment model](load-balancer-get-started-internet-classic-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-by-using-azure-powershell"></a><span data-ttu-id="11bea-110">Nasazení řešení hello pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="11bea-110">Deploying hello solution by using Azure PowerShell</span></span>

<span data-ttu-id="11bea-111">Hello následující postupy popisují, jak toocreate internetové nástroj pro vyrovnávání zatížení pomocí Azure Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="11bea-111">hello following procedures explain how toocreate an Internet-facing load balancer by using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="11bea-112">S Azure Resource Manager, každého prostředku se vytvoří a konfigurovat individuálně a potom se spojí dohromady toocreate nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="11bea-112">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a load balancer.</span></span>

<span data-ttu-id="11bea-113">Musíte vytvořit a konfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="11bea-113">You must create and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="11bea-114">Konfigurace front-endových IP adres: obsahuje veřejné IP adresy pro příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="11bea-114">Front-end IP configuration: contains public IP (PIP) addresses for incoming network traffic.</span></span>
* <span data-ttu-id="11bea-115">Fond adres back-end: obsahuje síťová rozhraní (NIC) pro hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-115">Back-end address pool: contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="11bea-116">Pravidla Vyrovnávání zatížení: obsahuje pravidla, které mapují veřejný port na hello zatížení vyrovnávání tooa portu ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-116">Load-balancing rules: contains rules that map a public port on hello load balancer tooa port in hello back-end address pool.</span></span>
* <span data-ttu-id="11bea-117">Příchozí pravidla NAT: obsahuje pravidla, které mapují veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-117">Inbound NAT rules: contains rules that map a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="11bea-118">Sondy: obsahuje dostupnosti toocheck stavu sondy používané instancí virtuálního počítače ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-118">Probes: contains health probes used toocheck availability of virtual machine instances in hello back-end address pool.</span></span>

<span data-ttu-id="11bea-119">Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="11bea-119">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-powershell-toouse-resource-manager"></a><span data-ttu-id="11bea-120">Nastavení prostředí PowerShell toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11bea-120">Set up PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="11bea-121">Ujistěte se, že máte nejnovější verzi produkční hello hello modulu Azure Resource Manager pro prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="11bea-121">Make sure you have hello latest production version of hello Azure Resource Manager module for PowerShell:</span></span>

1. <span data-ttu-id="11bea-122">Přihlaste se tooAzure.</span><span class="sxs-lookup"><span data-stu-id="11bea-122">Sign in tooAzure.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="11bea-123">Po zobrazení výzvy zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="11bea-123">Enter your credentials when prompted.</span></span>

2. <span data-ttu-id="11bea-124">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-124">Check hello subscriptions for hello account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="11bea-125">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="11bea-125">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. <span data-ttu-id="11bea-126">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="11bea-126">Create a resource group.</span></span> <span data-ttu-id="11bea-127">(Tento krok přeskočte, pokud používáte některou ze stávajících skupin prostředků.)</span><span class="sxs-lookup"><span data-stu-id="11bea-127">(Skip this step if you're using an existing resource group.)</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="11bea-128">Vytvořit virtuální síť a veřejnou IP adresu pro fond hello front-end IP adres</span><span class="sxs-lookup"><span data-stu-id="11bea-128">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="11bea-129">Vytvořte podsíť a virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="11bea-129">Create a subnet and a virtual network.</span></span>

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. <span data-ttu-id="11bea-130">Vytvořit prostředek služby Azure veřejnou IP adresu, s názvem **PublicIP**, toobe používá front-endu fond IP adres s názvem DNS hello **loadbalancernrp.westus.cloudapp.azure.com**. následující příkaz hello používá hello Typ statického přidělování.</span><span class="sxs-lookup"><span data-stu-id="11bea-130">Create an Azure public IP address resource, named **PublicIP**, toobe used by a front-end IP pool with hello DNS name **loadbalancernrp.westus.cloudapp.azure.com**. hello following command uses hello static allocation type.</span></span>

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="11bea-131">Vyrovnávání zatížení Hello používá hello domény popisek hello veřejnou IP adresu jako předponu pro její plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="11bea-131">hello load balancer uses hello domain label of hello public IP as a prefix for its FQDN.</span></span> <span data-ttu-id="11bea-132">To se liší od modelu nasazení classic hello, který používá hello cloudové služby jako hello nástroj pro vyrovnávání zatížení plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="11bea-132">This is different from hello classic deployment model, which uses hello cloud service as hello load balancer FQDN.</span></span>
   > <span data-ttu-id="11bea-133">V tomto příkladu hello plně kvalifikovaný název domény je **loadbalancernrp.westus.cloudapp.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="11bea-133">In this example, hello FQDN is **loadbalancernrp.westus.cloudapp.azure.com**.</span></span>

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a><span data-ttu-id="11bea-134">Vytvoření front-endového fondu IP adres a back-endového fondu adres</span><span class="sxs-lookup"><span data-stu-id="11bea-134">Create a front-end IP pool and a back-end address pool</span></span>

1. <span data-ttu-id="11bea-135">Vytvořte front-end IP fond s názvem **LB front-endu** používající hello **PublicIp** prostředků.</span><span class="sxs-lookup"><span data-stu-id="11bea-135">Create a front-end IP pool named **LB-Frontend** that uses hello **PublicIp** resource.</span></span>

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. <span data-ttu-id="11bea-136">Vytvořte back-endový fond adres s názvem **LB-backend**.</span><span class="sxs-lookup"><span data-stu-id="11bea-136">Create a back-end address pool named **LB-backend**.</span></span>

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a><span data-ttu-id="11bea-137">Vytvoření pravidel překladu adres (NAT), pravidla nástroje pro vyrovnávání zatížení, testu a nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="11bea-137">Create NAT rules, a load balancer rule, a probe, and a load balancer</span></span>

<span data-ttu-id="11bea-138">Tento příklad vytvoří hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="11bea-138">This example creates hello following items:</span></span>

* <span data-ttu-id="11bea-139">Tootranslate pravidlo NAT všechny příchozí přenosy na portu 3441 tooport 3389</span><span class="sxs-lookup"><span data-stu-id="11bea-139">A NAT rule tootranslate all incoming traffic on port 3441 tooport 3389</span></span>
* <span data-ttu-id="11bea-140">Tootranslate pravidlo NAT všechny příchozí přenosy na portu 3442 tooport 3389</span><span class="sxs-lookup"><span data-stu-id="11bea-140">A NAT rule tootranslate all incoming traffic on port 3442 tooport 3389</span></span>
* <span data-ttu-id="11bea-141">Stav testu toocheck pravidlo hello na stránku s názvem **HealthProbe.aspx**</span><span class="sxs-lookup"><span data-stu-id="11bea-141">A probe rule toocheck hello health status on a page named **HealthProbe.aspx**</span></span>
* <span data-ttu-id="11bea-142">Toobalance pravidlo Vyrovnávání zatížení všechny příchozí přenosy na portu 80 tooport 80 na hello adresy ve fondu back-end hello</span><span class="sxs-lookup"><span data-stu-id="11bea-142">A load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool</span></span>
* <span data-ttu-id="11bea-143">Nástroj pro vyrovnávání zatížení, který používá všechny tyto objekty.</span><span class="sxs-lookup"><span data-stu-id="11bea-143">A load balancer that uses all these objects</span></span>

<span data-ttu-id="11bea-144">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="11bea-144">Use these steps:</span></span>

1. <span data-ttu-id="11bea-145">Vytvoření pravidla NAT hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-145">Create hello NAT rules.</span></span>

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. <span data-ttu-id="11bea-146">Vytvořte test stavu.</span><span class="sxs-lookup"><span data-stu-id="11bea-146">Create a health probe.</span></span> <span data-ttu-id="11bea-147">Existují dva způsoby tooconfigure sondu:</span><span class="sxs-lookup"><span data-stu-id="11bea-147">There are two ways tooconfigure a probe:</span></span>

    <span data-ttu-id="11bea-148">Test protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="11bea-148">HTTP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="11bea-149">Test protokolu TCP</span><span class="sxs-lookup"><span data-stu-id="11bea-149">TCP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. <span data-ttu-id="11bea-150">Vytvořte pravidlo nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="11bea-150">Create a load balancer rule.</span></span>

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. <span data-ttu-id="11bea-151">Vytvořte nástroj pro vyrovnávání zatížení hello pomocí hello dříve vytvořené objekty.</span><span class="sxs-lookup"><span data-stu-id="11bea-151">Create hello load balancer by using hello previously created objects.</span></span>

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a><span data-ttu-id="11bea-152">Vytvoření síťových rozhraní</span><span class="sxs-lookup"><span data-stu-id="11bea-152">Create NICs</span></span>

<span data-ttu-id="11bea-153">Vytvořit síťových rozhraní (nebo upravit existující) a přidružovat je tooNAT pravidel, pravidla nástroje pro vyrovnávání zatížení a sondy:</span><span class="sxs-lookup"><span data-stu-id="11bea-153">Create network interfaces (or modify existing ones) and then associate them tooNAT rules, load balancer rules, and probes:</span></span>

1. <span data-ttu-id="11bea-154">Získáte hello virtuální síť a podsíť virtuální sítě, kde síťové adaptéry hello musí toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="11bea-154">Get hello virtual network and a virtual network subnet, where hello NICs need toobe created.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. <span data-ttu-id="11bea-155">Vytvořit síťový adaptér s názvem **lb nic1 být**a přidružte ji k první pravidlo NAT hello a fond back-end adres první (a pouze) hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-155">Create a NIC named **lb-nic1-be**, and associate it with hello first NAT rule and hello first (and only) back-end address pool.</span></span>

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. <span data-ttu-id="11bea-156">Vytvořit síťový adaptér s názvem **lb nic2 být**a přidružte ji k hello druhé pravidlo NAT a fond back-end adres první (a pouze) hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-156">Create a NIC named **lb-nic2-be**, and associate it with hello second NAT rule and hello first (and only) back-end address pool.</span></span>

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. <span data-ttu-id="11bea-157">Zkontrolujte síťové adaptéry hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-157">Check hello NICs.</span></span>

        $backendnic1

    <span data-ttu-id="11bea-158">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="11bea-158">Expected output:</span></span>

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. <span data-ttu-id="11bea-159">Použití hello `Add-AzureRmVMNetworkInterface` rutiny tooassign hello síťové adaptéry toodifferent virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="11bea-159">Use hello `Add-AzureRmVMNetworkInterface` cmdlet tooassign hello NICs toodifferent VMs.</span></span>

## <a name="create-a-virtual-machine"></a><span data-ttu-id="11bea-160">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="11bea-160">Create a virtual machine</span></span>

<span data-ttu-id="11bea-161">Pokyny k vytvoření virtuálního počítače a přiřazení síťového rozhraní najdete v tématu [Vytvoření virtuálního počítače Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="11bea-161">For guidance on creating a virtual machine and assigning a NIC, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

## <a name="add-hello-network-interface-toohello-load-balancer"></a><span data-ttu-id="11bea-162">Přidat hello síťové rozhraní toohello nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="11bea-162">Add hello network interface toohello load balancer</span></span>

1. <span data-ttu-id="11bea-163">Nástroj pro vyrovnávání zatížení hello načíst z Azure.</span><span class="sxs-lookup"><span data-stu-id="11bea-163">Retrieve hello load balancer from Azure.</span></span>

    <span data-ttu-id="11bea-164">Načte prostředek pro vyrovnávání zatížení hello do proměnné (Pokud jste to neudělali, ještě).</span><span class="sxs-lookup"><span data-stu-id="11bea-164">Load hello load balancer resource into a variable (if you haven't done that yet).</span></span> <span data-ttu-id="11bea-165">Proměnná Hello se nazývá **$lb**. Hello použijte stejné názvy hello prostředek pro vyrovnávání zatížení, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="11bea-165">hello variable is called **$lb**. Use hello same names from hello load balancer resource that you created earlier.</span></span>

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. <span data-ttu-id="11bea-166">Načíst proměnnou tooa hello konfigurace back-end.</span><span class="sxs-lookup"><span data-stu-id="11bea-166">Load hello back-end configuration tooa variable.</span></span>

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. <span data-ttu-id="11bea-167">Zatížení hello už vytvořené síťové rozhraní do proměnné.</span><span class="sxs-lookup"><span data-stu-id="11bea-167">Load hello already created network interface into a variable.</span></span> <span data-ttu-id="11bea-168">Název proměnné Hello je **$nic**.</span><span class="sxs-lookup"><span data-stu-id="11bea-168">hello variable name is **$nic**.</span></span> <span data-ttu-id="11bea-169">Hello název síťového rozhraní je hello stejný jako ten, z hello předchozího příkladu.</span><span class="sxs-lookup"><span data-stu-id="11bea-169">hello network interface name is hello same one from hello earlier example.</span></span>

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. <span data-ttu-id="11bea-170">Změna konfigurace back-end hello hello síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="11bea-170">Change hello back-end configuration on hello network interface.</span></span>

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. <span data-ttu-id="11bea-171">Uložte objekt rozhraní sítě hello.</span><span class="sxs-lookup"><span data-stu-id="11bea-171">Save hello network interface object.</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    <span data-ttu-id="11bea-172">Po přidání fondu back-end pro vyrovnávání zatížení toohello síťové rozhraní začne přijímá síťový provoz na základě hello pravidel Vyrovnávání zatížení pro tento prostředek pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="11bea-172">After a network interface is added toohello load balancer back-end pool, it starts receiving network traffic based on hello load-balancing rules for that load balancer resource.</span></span>

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="11bea-173">Aktualizace stávajícího nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="11bea-173">Update an existing load balancer</span></span>

1. <span data-ttu-id="11bea-174">Pomocí hello vyrovnávání zátěže z předchozího příkladu Dobrý den, přiřazení proměnné objektu toohello nástroje pro vyrovnávání zatížení **$slb** pomocí `Get-AzureLoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="11bea-174">By using hello load balancer from hello earlier example, assign a load balancer object toohello variable **$slb** by using `Get-AzureLoadBalancer`.</span></span>

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. <span data-ttu-id="11bea-175">V následujícím příkladu hello přidat příchozí pravidlo NAT--pomocí portu 81 hello front-end fondu a portu 8181 pro fond back-end hello – tooan existující nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="11bea-175">In hello following example, you add an inbound NAT rule--by using port 81 in hello front-end pool and port 8181 for hello back-end pool--tooan existing load balancer.</span></span>

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. <span data-ttu-id="11bea-176">Uložte novou konfiguraci hello pomocí `Set-AzureLoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="11bea-176">Save hello new configuration by using `Set-AzureLoadBalancer`.</span></span>

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a><span data-ttu-id="11bea-177">Odebrání nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="11bea-177">Remove a load balancer</span></span>

<span data-ttu-id="11bea-178">Použijte příkaz hello `Remove-AzureLoadBalancer` toodelete Vyrovnávání zatížení dříve vytvořenou s názvem **NRP-LB** ve skupině prostředků s názvem **NRP-RG**.</span><span class="sxs-lookup"><span data-stu-id="11bea-178">Use hello command `Remove-AzureLoadBalancer` toodelete a previously created load balancer named **NRP-LB** in a resource group called **NRP-RG**.</span></span>

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> <span data-ttu-id="11bea-179">Můžete použít volitelný přepínač hello **-Force** tooavoid hello řádku pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="11bea-179">You can use hello optional switch **-Force** tooavoid hello prompt for deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11bea-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11bea-180">Next steps</span></span>

[<span data-ttu-id="11bea-181">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="11bea-181">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="11bea-182">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="11bea-182">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="11bea-183">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="11bea-183">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
