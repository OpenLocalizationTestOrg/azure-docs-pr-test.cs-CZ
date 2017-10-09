---
title: "aaaCreate směřujících Internetu pro vyrovnávání zátěže - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate k Internetu přístupných pro vyrovnávání zatížení v Resource Manager pomocí rozhraní příkazového řádku Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: cadb5edb3b4a4e2f0813109d027eaafdc7ef7303
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-load-balancer-using-hello-azure-cli"></a><span data-ttu-id="42762-103">Vytváření k Internetu pro vyrovnávání zatížení pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="42762-103">Creating an internet load balancer using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="42762-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="42762-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="42762-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="42762-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="42762-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="42762-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="42762-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="42762-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="42762-108">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="42762-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="42762-109">Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí klasického nasazení](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="42762-109">You can also [Learn how toocreate an Internet facing load balancer using classic deployment](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-using-hello-azure-cli"></a><span data-ttu-id="42762-110">Nasazení řešení hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="42762-110">Deploying hello solution using hello Azure CLI</span></span>

<span data-ttu-id="42762-111">Hello následující kroky ukazují, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí rozhraní příkazového řádku Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="42762-111">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="42762-112">S Azure Resource Manager každý prostředek je vytvořen a nakonfigurován individuálně, potom připravili toocreate prostředku.</span><span class="sxs-lookup"><span data-stu-id="42762-112">With Azure Resource Manager each resource is created and configured individually, then put together toocreate a resource.</span></span>

<span data-ttu-id="42762-113">Musíte vytvořit a konfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="42762-113">You must create and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="42762-114">Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="42762-114">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="42762-115">Fond adres back-end – obsahuje síťová rozhraní (NIC) pro hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="42762-115">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="42762-116">Pravidla pro vyrovnávání zatížení – obsahuje pravidla mapování veřejný port tooport nástroje pro vyrovnávání zatížení hello ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="42762-116">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="42762-117">Příchozí pravidla NAT – obsahuje pravidla mapování veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="42762-117">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="42762-118">Sondy – obsahuje dostupnosti toocheck stavu sondy používané instancí virtuálních počítačů ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="42762-118">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="42762-119">Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="42762-119">For more information see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-toouse-resource-manager"></a><span data-ttu-id="42762-120">Nastavení rozhraní příkazového řádku toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="42762-120">Set up CLI toouse Resource Manager</span></span>

1. <span data-ttu-id="42762-121">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="42762-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="42762-122">Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="42762-122">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
        azure config mode arm
    ```

    <span data-ttu-id="42762-123">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="42762-123">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="42762-124">Vytvořit virtuální síť a veřejnou IP adresu pro fond hello front-end IP adres</span><span class="sxs-lookup"><span data-stu-id="42762-124">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="42762-125">Vytvořit virtuální síť (VNet) s názvem *NRPVnet* v umístění východní USA hello pomocí skupinu prostředků s názvem *NRPRG*.</span><span class="sxs-lookup"><span data-stu-id="42762-125">Create a virtual network (VNet) named *NRPVnet* in hello East US location using a resource group named *NRPRG*.</span></span>

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    <span data-ttu-id="42762-126">Ve virtuální síti *NRPVnet* vytvořte podsíť s názvem *NRPVnetSubnet* s blokem CIDR 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="42762-126">Create a subnet named *NRPVnetSubnet* with a CIDR block of 10.0.0.0/24 in *NRPVnet*.</span></span>

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. <span data-ttu-id="42762-127">Vytvoření veřejné IP adresy s názvem *NRPPublicIP* toobe používá front-endu fond IP adres s názvem DNS *loadbalancernrp.eastus.cloudapp.azure.com*. následující příkaz hello používá hello statického přidělování typ a časový limit nečinnosti 4 minuty.</span><span class="sxs-lookup"><span data-stu-id="42762-127">Create a public IP address named *NRPPublicIP* toobe used by a front-end IP pool with DNS name *loadbalancernrp.eastus.cloudapp.azure.com*. hello command below uses hello static allocation type and idle timeout of 4 minutes.</span></span>

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="42762-128">Nástroj pro vyrovnávání zatížení Hello použije hello domény popisek hello veřejnou IP adresu jako jeho plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="42762-128">hello load balancer will use hello domain label of hello public IP as its FQDN.</span></span> <span data-ttu-id="42762-129">Tuto změnu z klasického nasazení, který používá hello cloudové služby jako hello nástroj pro vyrovnávání zatížení plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="42762-129">This a change from classic deployment, which uses hello cloud service as hello load balancer Fully Qualified Domain Name (FQDN).</span></span>
   > <span data-ttu-id="42762-130">V tomto příkladu hello plně kvalifikovaný název domény je *loadbalancernrp.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="42762-130">In this example, hello FQDN is *loadbalancernrp.eastus.cloudapp.azure.com*.</span></span>

## <a name="create-a-load-balancer"></a><span data-ttu-id="42762-131">Vytvoření nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42762-131">Create a load balancer</span></span>

<span data-ttu-id="42762-132">Hello následující příkaz vytvoří nástroj pro vyrovnávání zatížení s názvem *NRPlb* v hello *NRPRG* skupiny prostředků v hello *východní USA* umístění Azure.</span><span class="sxs-lookup"><span data-stu-id="42762-132">hello following command creates a load balancer named *NRPlb* in hello *NRPRG* resource group in hello *East US* Azure location.</span></span>

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a><span data-ttu-id="42762-133">Vytvoření front-endového fondu IP adres a back-endového fondu adres</span><span class="sxs-lookup"><span data-stu-id="42762-133">Create a front-end IP pool and a backend address pool</span></span>
<span data-ttu-id="42762-134">Tento příklad ukazuje, jak toocreate hello front-end IP fond, který obdrží hello příchozí síťový provoz na dobrý den, nástroj pro vyrovnávání zatížení a hello kde hello front-end fondu odešle hello skupinu s vyrovnáváním zatížení síťového provozu fond back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="42762-134">This example demonstrates how toocreate hello front-end IP pool that receives hello incoming network traffic on hello load balancer and hello backend IP pool where hello front-end pool sends hello load balanced network traffic.</span></span>

1. <span data-ttu-id="42762-135">Vytvořte front-end IP fond přidružení hello veřejnou IP adresu, vytvořili v předchozím kroku hello a nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="42762-135">Create a front-end IP pool associating hello public IP created in hello previous step and hello load balancer.</span></span>

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. <span data-ttu-id="42762-136">Nastavení fondu back-end adresy použít tooreceive příchozí provoz z fondu IP front-endu hello.</span><span class="sxs-lookup"><span data-stu-id="42762-136">Set up a back-end address pool used tooreceive incoming traffic from hello front-end IP pool.</span></span>

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a><span data-ttu-id="42762-137">Vytvoření pravidel nástroje pro vyrovnávání zatížení, pravidel překladu adres (NAT) a testu</span><span class="sxs-lookup"><span data-stu-id="42762-137">Create LB rules, NAT rules, and probe</span></span>

<span data-ttu-id="42762-138">Tento příklad vytvoří hello následující položky.</span><span class="sxs-lookup"><span data-stu-id="42762-138">This example creates hello following items.</span></span>

* <span data-ttu-id="42762-139">zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 21 tooport 22<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="42762-139">a NAT rule tootranslate all incoming traffic on port 21 tooport 22<sup>1</sup></span></span>
* <span data-ttu-id="42762-140">zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 23 tooport 22</span><span class="sxs-lookup"><span data-stu-id="42762-140">a NAT rule tootranslate all incoming traffic on port 23 tooport 22</span></span>
* <span data-ttu-id="42762-141">toobalance pravidlo Vyrovnávání zatížení všechny příchozí přenosy na portu 80 tooport 80 na hello adresy ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="42762-141">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>
* <span data-ttu-id="42762-142">Stav testu toocheck pravidlo hello na stránku s názvem *HealthProbe.aspx*.</span><span class="sxs-lookup"><span data-stu-id="42762-142">a probe rule toocheck hello health status on a page named *HealthProbe.aspx*.</span></span>

<span data-ttu-id="42762-143"><sup>1</sup> pravidel NAT, která jsou přidružená tooa instance konkrétní virtuální počítač za nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="42762-143"><sup>1</sup> NAT rules are associated tooa specific virtual machine instance behind hello load balancer.</span></span> <span data-ttu-id="42762-144">Hello síťový provoz na portu 21 přicházejících se odesílají na port 22, které jsou spojené s tímto pravidlem NAT tooa konkrétní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="42762-144">hello network traffic arriving on port 21 is sent tooa specific virtual machine on port 22 associated with this NAT rule.</span></span> <span data-ttu-id="42762-145">Pro pravidlo překladu adres (NAT) je nutné zadat protokol (UDP nebo TCP).</span><span class="sxs-lookup"><span data-stu-id="42762-145">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="42762-146">Oba protokoly nemůže být přiřazen toohello stejný port.</span><span class="sxs-lookup"><span data-stu-id="42762-146">Both protocols can't be assigned toohello same port.</span></span>

1. <span data-ttu-id="42762-147">Vytvoření pravidla NAT hello.</span><span class="sxs-lookup"><span data-stu-id="42762-147">Create hello NAT rules.</span></span>

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. <span data-ttu-id="42762-148">Vytvořte pravidlo nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42762-148">Create a load balancer rule.</span></span>

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. <span data-ttu-id="42762-149">Vytvořte test stavu.</span><span class="sxs-lookup"><span data-stu-id="42762-149">Create a health probe.</span></span>

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. <span data-ttu-id="42762-150">Zkontrolujte nastavení.</span><span class="sxs-lookup"><span data-stu-id="42762-150">Check your settings.</span></span>

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    <span data-ttu-id="42762-151">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="42762-151">Expected output:</span></span>

        info:    Executing command network lb show
        + Looking up hello load balancer "nrplb"
        + Looking up hello public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a><span data-ttu-id="42762-152">Vytvoření síťových rozhraní</span><span class="sxs-lookup"><span data-stu-id="42762-152">Create NICs</span></span>

<span data-ttu-id="42762-153">Třeba toocreate síťových adaptérů (nebo upravit existující) a přidružovat je sondy, tooNAT pravidla a pravidla nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42762-153">You need toocreate NICs (or modify existing ones) and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="42762-154">Vytvořit síťový adaptér s názvem *lb nic1 být*a přidružte ji k hello *rdp1* NAT pravidlo a hello *NRPbackendpool* fond adres back-end.</span><span class="sxs-lookup"><span data-stu-id="42762-154">Create a NIC named *lb-nic1-be*, and associate it with hello *rdp1* NAT rule, and hello *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    <span data-ttu-id="42762-155">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="42762-155">Expected output:</span></span>

        info:    Executing command network nic create
        + Looking up hello network interface "lb-nic1-be"
        + Looking up hello subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up hello network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. <span data-ttu-id="42762-156">Vytvořit síťový adaptér s názvem *lb nic2 být*a přidružte ji k hello *rdp2* NAT pravidlo a hello *NRPbackendpool* fond adres back-end.</span><span class="sxs-lookup"><span data-stu-id="42762-156">Create a NIC named *lb-nic2-be*, and associate it with hello *rdp2* NAT rule, and hello *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. <span data-ttu-id="42762-157">Vytvoření virtuálního počítače (VM) s názvem *web1*a přidružte ji k hello síťový adaptér s názvem *lb nic1 být*.</span><span class="sxs-lookup"><span data-stu-id="42762-157">Create a virtual machine (VM) named *web1*, and associate it with hello NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="42762-158">Účet úložiště s názvem *web1nrp* byl vytvořen před spuštěním příkazu hello níže.</span><span class="sxs-lookup"><span data-stu-id="42762-158">A storage account called *web1nrp* was created before running hello command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="42762-159">Virtuální počítače v zatížení vyrovnávání nutné toobe v hello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="42762-159">VMs in a load balancer need toobe in hello same availability set.</span></span> <span data-ttu-id="42762-160">Použití `azure availset create` toocreate sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="42762-160">Use `azure availset create` toocreate an availability set.</span></span>

    <span data-ttu-id="42762-161">výstup Hello by měl vypadat podobně jako toohello následující:</span><span class="sxs-lookup"><span data-stu-id="42762-161">hello output should be similar toohello following:</span></span>

        info:    Executing command vm create
        + Looking up hello VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using hello VM Size "Standard_A1"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account web1nrp
        + Looking up hello availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up hello NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in hello NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > <span data-ttu-id="42762-162">informační zpráva Hello **to je síťový adaptér bez publicIP nakonfigurované** očekává se, protože hello vytvořit síťovou kartu pro vyrovnávání zatížení hello připojení tooInternet pomocí hello zatížení vyrovnávání veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="42762-162">hello informational message **This is a NIC without publicIP configured** is expected since hello NIC created for hello load balancer connecting tooInternet using hello load balancer public IP address.</span></span>

    <span data-ttu-id="42762-163">Od hello *lb nic1 být* síťový adaptér je přidružen hello *rdp1* pravidla NAT, se můžete připojit příliš*web1* pomocí protokolu RDP přes port 3441 na Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="42762-163">Since hello *lb-nic1-be* NIC is associated with hello *rdp1* NAT rule, you can connect too*web1* using RDP through port 3441 on hello load balancer.</span></span>

4. <span data-ttu-id="42762-164">Vytvoření virtuálního počítače (VM) s názvem *web2*a přidružte ji k hello síťový adaptér s názvem *lb nic2 být*.</span><span class="sxs-lookup"><span data-stu-id="42762-164">Create a virtual machine (VM) named *web2*, and associate it with hello NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="42762-165">Účet úložiště s názvem *web1nrp* byl vytvořen před spuštěním příkazu hello níže.</span><span class="sxs-lookup"><span data-stu-id="42762-165">A storage account called *web1nrp* was created before running hello command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="42762-166">Aktualizace stávajícího nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42762-166">Update an existing load balancer</span></span>
<span data-ttu-id="42762-167">Můžete přidat pravidla odkazující na stávající nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="42762-167">You can add rules referencing an existing load balancer.</span></span> <span data-ttu-id="42762-168">V dalším příkladu hello je přidat nové pravidlo Vyrovnávání zatížení tooan existující nástroj pro vyrovnávání zatížení **NRPlb**</span><span class="sxs-lookup"><span data-stu-id="42762-168">In hello next example, a new load balancer rule is added tooan existing load balancer **NRPlb**</span></span>

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="42762-169">Odstranění nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42762-169">Delete a load balancer</span></span>
<span data-ttu-id="42762-170">Použijte následující příkaz tooremove nástroj pro vyrovnávání zatížení hello:</span><span class="sxs-lookup"><span data-stu-id="42762-170">Use hello following command tooremove a load balancer:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a><span data-ttu-id="42762-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42762-171">Next steps</span></span>
[<span data-ttu-id="42762-172">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42762-172">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="42762-173">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42762-173">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="42762-174">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="42762-174">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
