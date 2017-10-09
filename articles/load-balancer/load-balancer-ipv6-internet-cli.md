---
title: "Služba Vyrovnávání zatížení aaaCreate směřujících Internetu s IPv6 – rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate k Internetu přístupných pro vyrovnávání zatížení s IPv6 v Azure Resource Manager pomocí rozhraní příkazového řádku Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "protokol IPv6, nástroje pro vyrovnávání zatížení azure, duálním zásobníkem, veřejnou IP adresu, nativní protokol ipv6, mobilní, iot"
ms.assetid: a1957c9c-9c1d-423e-9d5c-d71449bc1f37
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 7ff75ac90d74a74e3d0c27672b36fbd955a086a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-hello-azure-cli"></a><span data-ttu-id="1daa7-104">Vytvoření internetové Vyrovnávání zatížení s IPv6 v správce Azure Resource Manager pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1daa7-104">Create an Internet facing load balancer with IPv6 in Azure Resource Manager using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1daa7-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1daa7-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="1daa7-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1daa7-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="1daa7-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="1daa7-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="1daa7-108">Azure Load Balancer je nástroj pro vyrovnávání zatížení úrovně 4 (TCP, UDP).</span><span class="sxs-lookup"><span data-stu-id="1daa7-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="1daa7-109">Nástroj pro vyrovnávání zatížení Hello poskytuje vysokou dostupnost distribucí příchozí komunikaci mezi instance pořádku služby ve cloudových službách nebo virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1daa7-109">hello load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="1daa7-110">Azure Load Balancer můžete také tyto služby prezentovat na více portech, více IP adresách nebo obojím.</span><span class="sxs-lookup"><span data-stu-id="1daa7-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="1daa7-111">Příklad scénáře nasazení</span><span class="sxs-lookup"><span data-stu-id="1daa7-111">Example deployment scenario</span></span>

<span data-ttu-id="1daa7-112">Hello následující diagram znázorňuje řešení vyrovnávání zatížení hello nasazuje pomocí šablony příklad hello popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="1daa7-112">hello following diagram illustrates hello load balancing solution being deployed using hello example template described in this article.</span></span>

![Scénář nástroje pro vyrovnávání zatížení](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

<span data-ttu-id="1daa7-114">V tomto scénáři vytvoříte následující prostředky Azure hello:</span><span class="sxs-lookup"><span data-stu-id="1daa7-114">In this scenario you will create hello following Azure resources:</span></span>

* <span data-ttu-id="1daa7-115">dva virtuální počítače (VM)</span><span class="sxs-lookup"><span data-stu-id="1daa7-115">two virtual machines (VMs)</span></span>
* <span data-ttu-id="1daa7-116">rozhraní virtuální sítě pro každý virtuální počítač s oba protokoly IPv4 a IPv6 adresy přiřazené</span><span class="sxs-lookup"><span data-stu-id="1daa7-116">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>
* <span data-ttu-id="1daa7-117">Vyrovnávání zatížení straně Internetu s IPv4 a IPv6 veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="1daa7-117">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="1daa7-118">toothat sadu dostupnosti obsahuje hello dva virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="1daa7-118">an Availability Set toothat contains hello two VMs</span></span>
* <span data-ttu-id="1daa7-119">dva načíst vyrovnávání pravidla toomap hello veřejné VIP toohello privátní koncové body</span><span class="sxs-lookup"><span data-stu-id="1daa7-119">two load balancing rules toomap hello public VIPs toohello private endpoints</span></span>

## <a name="deploying-hello-solution-using-hello-azure-cli"></a><span data-ttu-id="1daa7-120">Nasazení řešení hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1daa7-120">Deploying hello solution using hello Azure CLI</span></span>

<span data-ttu-id="1daa7-121">Hello následující kroky ukazují, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí rozhraní příkazového řádku Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1daa7-121">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="1daa7-122">S Azure Resource Manager, každého prostředku se vytvoří a konfigurovat individuálně a potom se spojí dohromady toocreate prostředku.</span><span class="sxs-lookup"><span data-stu-id="1daa7-122">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a resource.</span></span>

<span data-ttu-id="1daa7-123">toodeploy nástroj pro vyrovnávání zatížení, můžete vytvořit a nakonfigurovat hello následující objekty:</span><span class="sxs-lookup"><span data-stu-id="1daa7-123">toodeploy a load balancer, you create and configure hello following objects:</span></span>

* <span data-ttu-id="1daa7-124">Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="1daa7-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="1daa7-125">Fond adres back-end – obsahuje síťová rozhraní (NIC) pro hello virtuální počítače tooreceive síťový provoz z nástroje pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="1daa7-125">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="1daa7-126">Pravidla pro vyrovnávání zatížení – obsahuje pravidla mapování veřejný port tooport nástroje pro vyrovnávání zatížení hello ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="1daa7-126">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="1daa7-127">Příchozí pravidla NAT – obsahuje pravidla mapování veřejný port na hello zatížení vyrovnávání tooa portu pro konkrétní virtuální počítač ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="1daa7-127">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="1daa7-128">Sondy – obsahuje dostupnosti toocheck stavu sondy používané instancí virtuálních počítačů ve fondu adres back-end hello.</span><span class="sxs-lookup"><span data-stu-id="1daa7-128">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="1daa7-129">Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="1daa7-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-your-cli-environment-toouse-azure-resource-manager"></a><span data-ttu-id="1daa7-130">Nastavení prostředí toouse vaše rozhraní příkazového řádku Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1daa7-130">Set up your CLI environment toouse Azure Resource Manager</span></span>

<span data-ttu-id="1daa7-131">V tomto příkladu používáme hello nástrojů příkazového řádku v příkazovém okně prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1daa7-131">For this example, we are running hello CLI tools in a PowerShell command window.</span></span> <span data-ttu-id="1daa7-132">Jsme nejsou pomocí rutin prostředí Azure PowerShell hello ale také používáme skriptování možnosti tooimprove čitelnost a opětovné používání Powershellu.</span><span class="sxs-lookup"><span data-stu-id="1daa7-132">We are not using hello Azure PowerShell cmdlets but we use PowerShell's scripting capabilities tooimprove readability and reuse.</span></span>

1. <span data-ttu-id="1daa7-133">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="1daa7-133">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="1daa7-134">Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu.</span><span class="sxs-lookup"><span data-stu-id="1daa7-134">Run hello **azure config mode** command tooswitch tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="1daa7-135">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="1daa7-135">Expected output:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="1daa7-136">Přihlaste se tooAzure a získejte seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="1daa7-136">Sign in tooAzure and get a list of subscriptions.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="1daa7-137">Zadejte vaše přihlašovací údaje Azure po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="1daa7-137">Enter your Azure credentials when prompted.</span></span>

    ```azurecli
    azure account list
    ```

    <span data-ttu-id="1daa7-138">Vyberte hello odběr, který chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="1daa7-138">Pick hello subscription you want toouse.</span></span> <span data-ttu-id="1daa7-139">Poznamenejte si Id předplatného hello hello další krok.</span><span class="sxs-lookup"><span data-stu-id="1daa7-139">Make note of hello subscription Id for hello next step.</span></span>

4. <span data-ttu-id="1daa7-140">Nastavení proměnných prostředí PowerShell pro použití s hello rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1daa7-140">Set up PowerShell variables for use with hello CLI commands.</span></span>

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a><span data-ttu-id="1daa7-141">Vytvoření skupiny prostředků, nástroj pro vyrovnávání zatížení, virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="1daa7-141">Create a resource group, a load balancer, a virtual network, and subnets</span></span>

1. <span data-ttu-id="1daa7-142">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1daa7-142">Create a resource group</span></span>

    ```azurecli
    azure group create $rgName $location
    ```

2. <span data-ttu-id="1daa7-143">Vytvoření nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1daa7-143">Create a load balancer</span></span>

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. <span data-ttu-id="1daa7-144">Vytvořte virtuální síť (VNet).</span><span class="sxs-lookup"><span data-stu-id="1daa7-144">Create a virtual network (VNet).</span></span>

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

    <span data-ttu-id="1daa7-145">Vytvořte dvě podsítě v této virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1daa7-145">Create two subnets in this VNet.</span></span>

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-hello-front-end-pool"></a><span data-ttu-id="1daa7-146">Vytvoření veřejné IP adresy pro front-end fond hello</span><span class="sxs-lookup"><span data-stu-id="1daa7-146">Create public IP addresses for hello front-end pool</span></span>

1. <span data-ttu-id="1daa7-147">Nastavení proměnných prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="1daa7-147">Set up hello PowerShell variables</span></span>

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. <span data-ttu-id="1daa7-148">Vytvoření veřejné IP hello front-end IP fondu adres.</span><span class="sxs-lookup"><span data-stu-id="1daa7-148">Create a public IP address hello front-end IP pool.</span></span>

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="1daa7-149">Nástroj pro vyrovnávání zatížení Hello používá hello domény popisek hello veřejnou IP adresu jako jeho plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="1daa7-149">hello load balancer uses hello domain label of hello public IP as its FQDN.</span></span> <span data-ttu-id="1daa7-150">Tuto změnu z klasického nasazení, který používá název cloudové služby hello jako hello nástroj pro vyrovnávání zatížení plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="1daa7-150">This a change from classic deployment, which uses hello cloud service name as hello load balancer FQDN.</span></span>
    > <span data-ttu-id="1daa7-151">V tomto příkladu hello plně kvalifikovaný název domény je *contoso09152016.southcentralus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="1daa7-151">In this example, hello FQDN is *contoso09152016.southcentralus.cloudapp.azure.com*.</span></span>

## <a name="create-front-end-and-back-end-pools"></a><span data-ttu-id="1daa7-152">Vytvořte front-end a back endové fondy</span><span class="sxs-lookup"><span data-stu-id="1daa7-152">Create front-end and back-end pools</span></span>

<span data-ttu-id="1daa7-153">Tento příklad vytvoří hello front-end IP fond, který obdrží hello příchozí síťový provoz na Vyrovnávání zatížení hello a kde hello front-end fondu odešle hello skupinu s vyrovnáváním zatížení síťového provozu fondu hello back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="1daa7-153">This example creates hello front-end IP pool that receives hello incoming network traffic on hello load balancer and hello back-end IP pool where hello front-end pool sends hello load balanced network traffic.</span></span>

1. <span data-ttu-id="1daa7-154">Nastavení proměnných prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="1daa7-154">Set up hello PowerShell variables</span></span>

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. <span data-ttu-id="1daa7-155">Vytvořte front-end IP fond přidružení hello veřejnou IP adresu, vytvořili v předchozím kroku hello a nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="1daa7-155">Create a front-end IP pool associating hello public IP created in hello previous step and hello load balancer.</span></span>

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-hello-probe-nat-rules-and-lb-rules"></a><span data-ttu-id="1daa7-156">Vytvoření testu hello, pravidla NAT a pravidel LB</span><span class="sxs-lookup"><span data-stu-id="1daa7-156">Create hello probe, NAT rules, and LB rules</span></span>

<span data-ttu-id="1daa7-157">Tento příklad vytvoří hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="1daa7-157">This example creates hello following items:</span></span>

* <span data-ttu-id="1daa7-158">toocheck pravidla testu pro připojení k tooTCP port 80</span><span class="sxs-lookup"><span data-stu-id="1daa7-158">a probe rule toocheck for connectivity tooTCP port 80</span></span>
* <span data-ttu-id="1daa7-159">zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 3389 tooport 3389 pro protokol RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="1daa7-159">a NAT rule tootranslate all incoming traffic on port 3389 tooport 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="1daa7-160">zařízení NAT pravidlo tootranslate všechny příchozí přenosy na portu 3391 tooport 3389 pro protokol RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="1daa7-160">a NAT rule tootranslate all incoming traffic on port 3391 tooport 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="1daa7-161">toobalance pravidlo Vyrovnávání zatížení všechny příchozí přenosy na portu 80 tooport 80 na hello adresy ve fondu back-end hello.</span><span class="sxs-lookup"><span data-stu-id="1daa7-161">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>

<span data-ttu-id="1daa7-162"><sup>1</sup> pravidel NAT, která jsou přidružená tooa instance konkrétní virtuální počítač za nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="1daa7-162"><sup>1</sup> NAT rules are associated tooa specific virtual machine instance behind hello load balancer.</span></span> <span data-ttu-id="1daa7-163">Hello síťový provoz na portu 3389 přicházejících odešle toohello konkrétní virtuální počítač a port pravidla NAT hello přidružený.</span><span class="sxs-lookup"><span data-stu-id="1daa7-163">hello network traffic arriving on port 3389 is sent toohello specific virtual machine and port associated with hello NAT rule.</span></span> <span data-ttu-id="1daa7-164">Pro pravidlo překladu adres (NAT) je nutné zadat protokol (UDP nebo TCP).</span><span class="sxs-lookup"><span data-stu-id="1daa7-164">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="1daa7-165">Oba protokoly nemůže být přiřazen toohello stejný port.</span><span class="sxs-lookup"><span data-stu-id="1daa7-165">Both protocols can't be assigned toohello same port.</span></span>

1. <span data-ttu-id="1daa7-166">Nastavení proměnných prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="1daa7-166">Set up hello PowerShell variables</span></span>

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. <span data-ttu-id="1daa7-167">Vytvořit test paměti hello</span><span class="sxs-lookup"><span data-stu-id="1daa7-167">Create hello probe</span></span>

    <span data-ttu-id="1daa7-168">Hello následující příklad vytvoří sondou TCP, který kontroluje pro připojení k tooback-end port TCP 80 každých 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="1daa7-168">hello following example creates a TCP probe that checks for connectivity tooback-end TCP port 80 every 15 seconds.</span></span> <span data-ttu-id="1daa7-169">Ho označíte hello back-end prostředku není k dispozici po dvě po sobě jdoucích selhání.</span><span class="sxs-lookup"><span data-stu-id="1daa7-169">It will mark hello back-end resource unavailable after two consecutive failures.</span></span>

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. <span data-ttu-id="1daa7-170">Vytvoření příchozích pravidel NAT, které umožňují připojení RDP toohello back endové prostředky</span><span class="sxs-lookup"><span data-stu-id="1daa7-170">Create inbound NAT rules that allow RDP connections toohello back-end resources</span></span>

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. <span data-ttu-id="1daa7-171">Vytvořit pravidla, která odesílat provoz porty toodifferent back-end v závislosti na tom, které front-endu přijal žádost hello Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1daa7-171">Create a load balancer rules that send traffic toodifferent back-end ports depending on which front end received hello request</span></span>

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. <span data-ttu-id="1daa7-172">Zkontrolujte nastavení</span><span class="sxs-lookup"><span data-stu-id="1daa7-172">Check your settings</span></span>

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    <span data-ttu-id="1daa7-173">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="1daa7-173">Expected output:</span></span>

        info:    Executing command network lb show
        info:    Looking up hello load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show

## <a name="create-nics"></a><span data-ttu-id="1daa7-174">Vytvoření síťových rozhraní</span><span class="sxs-lookup"><span data-stu-id="1daa7-174">Create NICs</span></span>

<span data-ttu-id="1daa7-175">Vytvořte síťové adaptéry a přiřaďte je sondy, tooNAT pravidla a pravidla nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1daa7-175">Create NICs and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="1daa7-176">Nastavení proměnných prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="1daa7-176">Set up hello PowerShell variables</span></span>

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. <span data-ttu-id="1daa7-177">Vytvořte síťovou kartu pro každý back-end a přidejte konfigurace protokolu IPv6.</span><span class="sxs-lookup"><span data-stu-id="1daa7-177">Create a NIC for each back-end and add an IPv6 configuration.</span></span>

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-hello-back-end-vm-resources-and-attach-each-nic"></a><span data-ttu-id="1daa7-178">Vytvořte prostředky virtuálních počítačů hello back-end a připojte každý síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="1daa7-178">Create hello back-end VM resources and attach each NIC</span></span>

<span data-ttu-id="1daa7-179">toocreate virtuální počítače, musí mít účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1daa7-179">toocreate VMs, you must have a storage account.</span></span> <span data-ttu-id="1daa7-180">Pro vyrovnávání zatížení, musí virtuální počítače hello toobe členy skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="1daa7-180">For load balancing, hello VMs need toobe members of an availability set.</span></span> <span data-ttu-id="1daa7-181">Další informace o vytváření virtuálních počítačů najdete v tématu [vytvoření virtuálního počítače Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="1daa7-181">For more information about creating VMs, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="1daa7-182">Nastavení proměnných prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="1daa7-182">Set up hello PowerShell variables</span></span>

    ```powershell
    $storageAccountName = "ps08092016v6sa0"
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $disk1Name = "WindowsVMosDisk1"
    $disk2Name = "WindowsVMosDisk2"
    $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
    $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
    $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > <span data-ttu-id="1daa7-183">Tento příklad používá hello uživatelské jméno a heslo pro virtuální počítače hello ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="1daa7-183">This example uses hello username and password for hello VMs in clear text.</span></span> <span data-ttu-id="1daa7-184">Odpovídající musí dát pozor při použití pověření hello zrušte.</span><span class="sxs-lookup"><span data-stu-id="1daa7-184">Appropriate care must be taken when using credentials in hello clear.</span></span> <span data-ttu-id="1daa7-185">Bezpečnější způsob zpracování pověření v prostředí PowerShell, najdete v části hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1daa7-185">For a more secure method of handling credentials in PowerShell, see hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

2. <span data-ttu-id="1daa7-186">Vytvořit sadu dostupnosti a účet úložiště hello</span><span class="sxs-lookup"><span data-stu-id="1daa7-186">Create hello storage account and availability set</span></span>

    <span data-ttu-id="1daa7-187">Při vytváření hello virtuálních počítačů, může použít existující účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1daa7-187">You may use an existing storage account when you create hello VMs.</span></span> <span data-ttu-id="1daa7-188">Hello následující příkaz vytvoří nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1daa7-188">hello following command creates a new storage account.</span></span>

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

    <span data-ttu-id="1daa7-189">Dále vytvořte skupinu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="1daa7-189">Next, create hello availability set.</span></span>

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. <span data-ttu-id="1daa7-190">Vytvořit hello virtuální počítače s hello přidružené síťové karty</span><span class="sxs-lookup"><span data-stu-id="1daa7-190">Create hello virtual machines with hello associated NICs</span></span>

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a><span data-ttu-id="1daa7-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1daa7-191">Next steps</span></span>

[<span data-ttu-id="1daa7-192">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1daa7-192">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="1daa7-193">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1daa7-193">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="1daa7-194">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1daa7-194">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
