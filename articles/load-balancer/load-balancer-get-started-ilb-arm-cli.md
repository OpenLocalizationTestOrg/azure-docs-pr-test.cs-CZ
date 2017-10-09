---
title: "aaaCreate interní nástroj pro vyrovnávání - zatížení, rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate interní zátěže pomocí rozhraní příkazového řádku Azure ve službě Správce prostředků"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 3aea6fdb07600f0d661ec6b8ffc784b03380a127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-by-using-hello-azure-cli"></a><span data-ttu-id="a3bff-103">Vytvořit interní nástroj pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a3bff-103">Create an internal load balancer by using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a3bff-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a3bff-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="a3bff-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3bff-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="a3bff-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a3bff-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="a3bff-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="a3bff-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="a3bff-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a3bff-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="a3bff-109">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](load-balancer-get-started-ilb-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a3bff-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-solution-by-using-hello-azure-cli"></a><span data-ttu-id="a3bff-110">Nasazení řešení hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a3bff-110">Deploy hello solution by using hello Azure CLI</span></span>

<span data-ttu-id="a3bff-111">Hello následující kroky ukazují, jak toocreate internetové nástroj pro vyrovnávání zatížení pomocí rozhraní příkazového řádku Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a3bff-111">hello following steps show how toocreate an Internet-facing load balancer by using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="a3bff-112">S Azure Resource Manager, každého prostředku se vytvoří a konfigurovat individuálně a potom se spojí dohromady toocreate prostředku.</span><span class="sxs-lookup"><span data-stu-id="a3bff-112">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a resource.</span></span>

<span data-ttu-id="a3bff-113">Je třeba toocreate a nakonfigurovat hello následující objekty toodeploy nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="a3bff-113">You need toocreate and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="a3bff-114">**Konfigurace front-endových IP adres**: obsahuje veřejné IP adresy pro příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="a3bff-114">**Front-end IP configuration**: contains public IP addresses for incoming network traffic</span></span>
* <span data-ttu-id="a3bff-115">**Fond back-end adres**: obsahuje síťová rozhraní (NIC), které umožňují hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="a3bff-115">**Back-end address pool**: contains network interfaces (NICs) that enable hello virtual machines tooreceive network traffic from hello load balancer</span></span>
* <span data-ttu-id="a3bff-116">**Pravidla Vyrovnávání zatížení**: obsahuje pravidla, které mapují veřejný port tooport nástroje pro vyrovnávání zatížení hello ve fondu adres back-end hello</span><span class="sxs-lookup"><span data-stu-id="a3bff-116">**Load-balancing rules**: contains rules that map a public port on hello load balancer tooport in hello back-end address pool</span></span>
* <span data-ttu-id="a3bff-117">**Příchozí pravidla NAT**: obsahuje pravidla, které mapují veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello</span><span class="sxs-lookup"><span data-stu-id="a3bff-117">**Inbound NAT rules**: contains rules that map a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool</span></span>
* <span data-ttu-id="a3bff-118">**Sondy**: obsahuje sondy stavu, které jsou používané toocheck hello dostupnosti instancí virtuálních počítačů ve fondu adres back-end hello</span><span class="sxs-lookup"><span data-stu-id="a3bff-118">**Probes**: contains health probes that are used toocheck hello availability of virtual machines instances in hello back-end address pool</span></span>

<span data-ttu-id="a3bff-119">Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="a3bff-119">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-toouse-resource-manager"></a><span data-ttu-id="a3bff-120">Nastavení rozhraní příkazového řádku toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a3bff-120">Set up CLI toouse Resource Manager</span></span>

1. <span data-ttu-id="a3bff-121">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a3bff-121">If you have never used Azure CLI, see [Install and configure hello Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="a3bff-122">Postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="a3bff-122">Follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="a3bff-123">Spustit hello **azure konfigurace režim** příkaz režimu Manager tooResource tooswitch, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a3bff-123">Run hello **azure config mode** command tooswitch tooResource Manager mode, as follows:</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="a3bff-124">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="a3bff-124">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a><span data-ttu-id="a3bff-125">Vytvoření interního nástroje pro vyrovnávání zatížení krok za krokem</span><span class="sxs-lookup"><span data-stu-id="a3bff-125">Create an internal load balancer step by step</span></span>

1. <span data-ttu-id="a3bff-126">Přihlaste se tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a3bff-126">Sign in tooAzure.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="a3bff-127">Po zobrazení výzvy zadejte své přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="a3bff-127">When prompted, enter your Azure credentials.</span></span>

2. <span data-ttu-id="a3bff-128">Změna režimu Resource Manager tooAzure nástroje příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="a3bff-128">Change hello command tools tooAzure Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a><span data-ttu-id="a3bff-129">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a3bff-129">Create a resource group</span></span>

<span data-ttu-id="a3bff-130">Všechny prostředky v Azure Resource Manageru jsou přidruženy ke skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a3bff-130">All resources in Azure Resource Manager are associated with a resource group.</span></span> <span data-ttu-id="a3bff-131">Pokud jste tak ještě neučinili, vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a3bff-131">If you haven't done so yet, create a resource group.</span></span>

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a><span data-ttu-id="a3bff-132">Vytvoření sady interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="a3bff-132">Create an internal load balancer set</span></span>

1. <span data-ttu-id="a3bff-133">Vytvořte interní nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="a3bff-133">Create an internal load balancer</span></span>

    <span data-ttu-id="a3bff-134">V následujícím scénáři hello se vytvoří skupinu prostředků s názvem nrprg v oblasti Východ USA.</span><span class="sxs-lookup"><span data-stu-id="a3bff-134">In hello following scenario, a resource group named nrprg is created in East US region.</span></span>

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > <span data-ttu-id="a3bff-135">Všechny prostředky pro interní služby load balancer, jako je například virtuální sítě a podsítě virtuální sítě, musí být v hello stejnou skupinu prostředků a v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="a3bff-135">All resources for an internal load balancers, such as virtual networks and virtual network subnets, must be in hello same resource group and in hello same region.</span></span>

2. <span data-ttu-id="a3bff-136">Vytvořte front-end IP adresu pro vyrovnávání zatížení interní hello.</span><span class="sxs-lookup"><span data-stu-id="a3bff-136">Create a front-end IP address for hello internal load balancer.</span></span>

    <span data-ttu-id="a3bff-137">Hello IP adresu, která používáte musí být v rozsahu hello podsítě virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a3bff-137">hello IP address that you use must be within hello subnet range of your virtual network.</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. <span data-ttu-id="a3bff-138">Vytvořte fond back-end adres hello.</span><span class="sxs-lookup"><span data-stu-id="a3bff-138">Create hello back-end address pool.</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    <span data-ttu-id="a3bff-139">Po definování front-endové IP adresy a back-endového fondu adres můžete vytvořit pravidla nástroje pro vyrovnávání zatížení, pravidla příchozího překladu adres (NAT) a vlastní testy stavu.</span><span class="sxs-lookup"><span data-stu-id="a3bff-139">After you define a front-end IP address and a back-end address pool, you can create load balancer rules, inbound NAT rules, and customized health probes.</span></span>

4. <span data-ttu-id="a3bff-140">Vytvořte pravidlo Vyrovnávání zatížení nástroje pro vyrovnávání zatížení interní hello.</span><span class="sxs-lookup"><span data-stu-id="a3bff-140">Create a load balancer rule for hello internal load balancer.</span></span>

    <span data-ttu-id="a3bff-141">Pokud budete postupovat podle předchozích kroků hello, hello příkaz vytvoří pravidlo Vyrovnávání zatížení pro naslouchání tooport 1433 ve front-endu fondu hello a odesílání Vyrovnávání zatížení provozu toohello back-end fondu síťových adres, také používá port 1433.</span><span class="sxs-lookup"><span data-stu-id="a3bff-141">When you follow hello previous steps, hello command creates a load-balancer rule for listening tooport 1433 in hello front-end pool and sending load-balanced network traffic toohello back-end address pool, also using port 1433.</span></span>

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. <span data-ttu-id="a3bff-142">Vytvořte pravidla příchozího překladu adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="a3bff-142">Create inbound NAT rules.</span></span>

    <span data-ttu-id="a3bff-143">Příchozí pravidla NAT jsou koncové body používané toocreate, nástroji pro vyrovnávání zatížení, které přejděte instance tooa konkrétní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a3bff-143">Inbound NAT rules are used toocreate endpoints in a load balancer that go tooa specific virtual machine instance.</span></span> <span data-ttu-id="a3bff-144">předchozí kroky Hello vytvořit dvě pravidla NAT pro vzdálenou plochu.</span><span class="sxs-lookup"><span data-stu-id="a3bff-144">hello previous steps created two NAT rules  for remote desktop.</span></span>

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. <span data-ttu-id="a3bff-145">Vytvořte sondy stavu služby pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="a3bff-145">Create health probes for hello load balancer.</span></span>

    <span data-ttu-id="a3bff-146">Test stavu kontroluje všechny instance virtuálního počítače toomake se, že se mohou odesílat provoz sítě.</span><span class="sxs-lookup"><span data-stu-id="a3bff-146">A health probe checks all virtual machine instances toomake sure they can send network traffic.</span></span> <span data-ttu-id="a3bff-147">Hello instanci virtuálního počítače s kontroly selhání testu se odebere z nástroje pro vyrovnávání zatížení hello dokud přejde do režimu online a kontrolu testu Určuje, že je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="a3bff-147">hello virtual machine instance with failed probe checks is removed from hello load balancer until it goes back online and a probe check determines that it's healthy.</span></span>

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > <span data-ttu-id="a3bff-148">Platforma Microsoft Azure Hello používá statické veřejně směrovatelné IPv4 adresu pro různé scénáře pro správu.</span><span class="sxs-lookup"><span data-stu-id="a3bff-148">hello Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="a3bff-149">Hello IP adresa je 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="a3bff-149">hello IP address is 168.63.129.16.</span></span> <span data-ttu-id="a3bff-150">Tuto IP adresu by neměla blokovat žádná brána firewall, protože by to mohlo způsobit neočekávané chování.</span><span class="sxs-lookup"><span data-stu-id="a3bff-150">This IP address should not be blocked by any firewalls, because this can cause unexpected behavior.</span></span>
    > <span data-ttu-id="a3bff-151">S ohledem tooAzure interní Vyrovnávání zatížení se sledováním sondy z hello zatížení vyrovnávání toodetermine hello stav pro virtuální počítače v sadě Vyrovnávání zatížení sítě používá tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="a3bff-151">With respect tooAzure internal load balancing, this IP address is used by monitoring probes from hello load balancer toodetermine hello health state for virtual machines in a load-balanced set.</span></span> <span data-ttu-id="a3bff-152">Pokud skupina zabezpečení sítě je použité toorestrict provoz tooAzure virtuálních počítačů v sadu interně Vyrovnávání zatížení sítě nebo je použité tooa podsíť virtuální sítě, ujistěte se, zda pravidla zabezpečení sítě je přidána tooallow provoz z 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="a3bff-152">If a network security group is used toorestrict traffic tooAzure virtual machines in an internally load-balanced set or is applied tooa virtual network subnet, ensure that a network security rule is added tooallow traffic from 168.63.129.16.</span></span>

## <a name="create-nics"></a><span data-ttu-id="a3bff-153">Vytvoření síťových rozhraní</span><span class="sxs-lookup"><span data-stu-id="a3bff-153">Create NICs</span></span>

<span data-ttu-id="a3bff-154">Třeba toocreate síťových adaptérů (nebo upravit existující) a přidružovat je sondy, tooNAT pravidla a pravidla nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="a3bff-154">You need toocreate NICs (or modify existing ones) and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="a3bff-155">Vytvořte síťový adaptér s názvem *lb nic1 být*a přidružte ji k hello *rdp1* NAT pravidlo a hello *beilb* fond adres back-end.</span><span class="sxs-lookup"><span data-stu-id="a3bff-155">Create an NIC named *lb-nic1-be*, and then associate it with hello *rdp1* NAT rule and hello *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    <span data-ttu-id="a3bff-156">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="a3bff-156">Expected output:</span></span>

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

2. <span data-ttu-id="a3bff-157">Vytvořte síťový adaptér s názvem *lb nic2 být*a přidružte ji k hello *rdp2* NAT pravidlo a hello *beilb* fond adres back-end.</span><span class="sxs-lookup"><span data-stu-id="a3bff-157">Create an NIC named *lb-nic2-be*, and then associate it with hello *rdp2* NAT rule and hello *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. <span data-ttu-id="a3bff-158">Vytvoření virtuálního počítače s názvem *DB1*a přidružte ji k hello síťový adaptér s názvem *lb nic1 být*.</span><span class="sxs-lookup"><span data-stu-id="a3bff-158">Create a virtual machine named *DB1*, and then associate it with hello NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="a3bff-159">Účet úložiště s názvem *web1nrp* je vytvořen před hello následující příkaz spustí:</span><span class="sxs-lookup"><span data-stu-id="a3bff-159">A storage account called *web1nrp* is created before hello following command runs:</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > <span data-ttu-id="a3bff-160">Virtuální počítače v zatížení vyrovnávání nutné toobe v hello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a3bff-160">VMs in a load balancer need toobe in hello same availability set.</span></span> <span data-ttu-id="a3bff-161">Použití `azure availset create` toocreate sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a3bff-161">Use `azure availset create` toocreate an availability set.</span></span>

4. <span data-ttu-id="a3bff-162">Vytvoření virtuálního počítače (VM) s názvem *DB2*a přidružte ji k hello síťový adaptér s názvem *lb nic2 být*.</span><span class="sxs-lookup"><span data-stu-id="a3bff-162">Create a virtual machine (VM) named *DB2*, and then associate it with hello NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="a3bff-163">Účet úložiště s názvem *web1nrp* byl vytvořen před spuštěním hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="a3bff-163">A storage account called *web1nrp* was created before running hello following command.</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="a3bff-164">Odstranění nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="a3bff-164">Delete a load balancer</span></span>

<span data-ttu-id="a3bff-165">tooremove nástroj pro vyrovnávání zatížení hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a3bff-165">tooremove a load balancer, use hello following command:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a><span data-ttu-id="a3bff-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a3bff-166">Next steps</span></span>

[<span data-ttu-id="a3bff-167">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou</span><span class="sxs-lookup"><span data-stu-id="a3bff-167">Configure a load balancer distribution mode by using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="a3bff-168">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="a3bff-168">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

