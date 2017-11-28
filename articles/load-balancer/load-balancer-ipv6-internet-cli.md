---
title: "Vytvoření Vyrovnávání zatížení internetového s IPv6 – rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se vytvářet internetovým Vyrovnávání zatížení s IPv6 v správce Azure Resource Manager pomocí rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: efb4771800c42df544c3cc37d1d164045fdcaf3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a><span data-ttu-id="d1faf-104">Vytvoření internetové Vyrovnávání zatížení s IPv6 v správce Azure Resource Manager pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="d1faf-104">Create an Internet facing load balancer with IPv6 in Azure Resource Manager using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d1faf-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1faf-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="d1faf-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d1faf-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="d1faf-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="d1faf-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="d1faf-108">Azure Load Balancer je nástroj pro vyrovnávání zatížení úrovně 4 (TCP, UDP).</span><span class="sxs-lookup"><span data-stu-id="d1faf-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="d1faf-109">Nástroj pro vyrovnávání zatížení poskytuje vysokou dostupnost díky distribuci příchozích přenosů mezi instance služeb, které jsou v pořádku, v cloudových službách nebo virtuálních počítačích v sadě nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d1faf-109">The load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="d1faf-110">Azure Load Balancer můžete také tyto služby prezentovat na více portech, více IP adresách nebo obojím.</span><span class="sxs-lookup"><span data-stu-id="d1faf-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="d1faf-111">Příklad scénáře nasazení</span><span class="sxs-lookup"><span data-stu-id="d1faf-111">Example deployment scenario</span></span>

<span data-ttu-id="d1faf-112">Následující diagram znázorňuje řešení vyrovnávání zatížení nasazuje pomocí šablony příklad popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d1faf-112">The following diagram illustrates the load balancing solution being deployed using the example template described in this article.</span></span>

![Scénář nástroje pro vyrovnávání zatížení](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

<span data-ttu-id="d1faf-114">V tomto scénáři vytvoříte následující prostředky Azure:</span><span class="sxs-lookup"><span data-stu-id="d1faf-114">In this scenario you will create the following Azure resources:</span></span>

* <span data-ttu-id="d1faf-115">dva virtuální počítače (VM)</span><span class="sxs-lookup"><span data-stu-id="d1faf-115">two virtual machines (VMs)</span></span>
* <span data-ttu-id="d1faf-116">rozhraní virtuální sítě pro každý virtuální počítač s oba protokoly IPv4 a IPv6 adresy přiřazené</span><span class="sxs-lookup"><span data-stu-id="d1faf-116">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>
* <span data-ttu-id="d1faf-117">Vyrovnávání zatížení straně Internetu s IPv4 a IPv6 veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="d1faf-117">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="d1faf-118">Skupiny dostupnosti pro, který obsahuje dva virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="d1faf-118">an Availability Set to that contains the two VMs</span></span>
* <span data-ttu-id="d1faf-119">dvě pravidla vyrovnávání mapovat veřejné virtuální privátní koncové body zatížení</span><span class="sxs-lookup"><span data-stu-id="d1faf-119">two load balancing rules to map the public VIPs to the private endpoints</span></span>

## <a name="deploying-the-solution-using-the-azure-cli"></a><span data-ttu-id="d1faf-120">Nasazení řešení pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="d1faf-120">Deploying the solution using the Azure CLI</span></span>

<span data-ttu-id="d1faf-121">Následující postup ukazuje, jak vytvořit internetový nástroj pro vyrovnávání zatížení pomocí Azure Resource Manageru s rozhraním příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d1faf-121">The following steps show how to create an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="d1faf-122">S Azure Resource Managerem se jednotlivé prostředky vytvoří a nakonfigurují zvlášť, následně se spojí dohromady a vytvoří prostředek.</span><span class="sxs-lookup"><span data-stu-id="d1faf-122">With Azure Resource Manager, each resource is created and configured individually, and then put together to create a resource.</span></span>

<span data-ttu-id="d1faf-123">Pokud chcete nasadit nástroj pro vyrovnávání zatížení, vytvořte a nakonfigurujte následující objekty:</span><span class="sxs-lookup"><span data-stu-id="d1faf-123">To deploy a load balancer, you create and configure the following objects:</span></span>

* <span data-ttu-id="d1faf-124">Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="d1faf-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="d1faf-125">Back-endový fond adres – obsahuje síťová rozhraní, pomocí kterých virtuální počítače přijímají síťový provoz z nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d1faf-125">Back-end address pool - contains network interfaces (NICs) for the virtual machines to receive network traffic from the load balancer.</span></span>
* <span data-ttu-id="d1faf-126">Pravidla vyrovnávání zatížení – obsahuje pravidla mapující veřejný port v nástroji pro vyrovnávání zatížení na port v back-endovém fondu adres.</span><span class="sxs-lookup"><span data-stu-id="d1faf-126">Load balancing rules - contains rules mapping a public port on the load balancer to port in the back-end address pool.</span></span>
* <span data-ttu-id="d1faf-127">Pravidla příchozího překladu adres (NAT) – obsahuje pravidla mapující veřejný port v nástroji pro vyrovnávání zatížení na port konkrétního virtuálního počítače v back-endovém fondu adres.</span><span class="sxs-lookup"><span data-stu-id="d1faf-127">Inbound NAT rules - contains rules mapping a public port on the load balancer to a port for a specific virtual machine in the back-end address pool.</span></span>
* <span data-ttu-id="d1faf-128">Testy – obsahuje testy stavu sloužící ke kontrole dostupnosti instancí virtuálních počítačů v back-endovém fondu adres.</span><span class="sxs-lookup"><span data-stu-id="d1faf-128">Probes - contains health probes used to check availability of virtual machines instances in the back-end address pool.</span></span>

<span data-ttu-id="d1faf-129">Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="d1faf-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a><span data-ttu-id="d1faf-130">Nastavení prostředí rozhraní příkazového řádku pomocí Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="d1faf-130">Set up your CLI environment to use Azure Resource Manager</span></span>

<span data-ttu-id="d1faf-131">V tomto příkladu používáme nástrojů příkazového řádku v příkazovém okně prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d1faf-131">For this example, we are running the CLI tools in a PowerShell command window.</span></span> <span data-ttu-id="d1faf-132">Jsme nejsou pomocí rutin prostředí Azure PowerShell, ale možnosti skriptování Powershellu využijeme ke zlepšení čitelnosti a opakované použití.</span><span class="sxs-lookup"><span data-stu-id="d1faf-132">We are not using the Azure PowerShell cmdlets but we use PowerShell's scripting capabilities to improve readability and reuse.</span></span>

1. <span data-ttu-id="d1faf-133">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d1faf-133">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="d1faf-134">Spustit **azure konfigurace režim** příkaz Přepnout do režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d1faf-134">Run the **azure config mode** command to switch to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="d1faf-135">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d1faf-135">Expected output:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="d1faf-136">Přihlaste se k Azure a získat seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="d1faf-136">Sign in to Azure and get a list of subscriptions.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="d1faf-137">Zadejte vaše přihlašovací údaje Azure po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="d1faf-137">Enter your Azure credentials when prompted.</span></span>

    ```azurecli
    azure account list
    ```

    <span data-ttu-id="d1faf-138">Vyberte odběr, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="d1faf-138">Pick the subscription you want to use.</span></span> <span data-ttu-id="d1faf-139">Poznamenejte si Id předplatného pro další krok.</span><span class="sxs-lookup"><span data-stu-id="d1faf-139">Make note of the subscription Id for the next step.</span></span>

4. <span data-ttu-id="d1faf-140">Nastavení proměnných prostředí PowerShell pro použití s příkazy rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d1faf-140">Set up PowerShell variables for use with the CLI commands.</span></span>

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

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a><span data-ttu-id="d1faf-141">Vytvoření skupiny prostředků, nástroj pro vyrovnávání zatížení, virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="d1faf-141">Create a resource group, a load balancer, a virtual network, and subnets</span></span>

1. <span data-ttu-id="d1faf-142">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="d1faf-142">Create a resource group</span></span>

    ```azurecli
    azure group create $rgName $location
    ```

2. <span data-ttu-id="d1faf-143">Vytvoření nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d1faf-143">Create a load balancer</span></span>

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. <span data-ttu-id="d1faf-144">Vytvořte virtuální síť (VNet).</span><span class="sxs-lookup"><span data-stu-id="d1faf-144">Create a virtual network (VNet).</span></span>

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

    <span data-ttu-id="d1faf-145">Vytvořte dvě podsítě v této virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d1faf-145">Create two subnets in this VNet.</span></span>

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a><span data-ttu-id="d1faf-146">Vytvoření veřejné IP adresy pro front-end fond</span><span class="sxs-lookup"><span data-stu-id="d1faf-146">Create public IP addresses for the front-end pool</span></span>

1. <span data-ttu-id="d1faf-147">Nastavení proměnných prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1faf-147">Set up the PowerShell variables</span></span>

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. <span data-ttu-id="d1faf-148">Vytvoření veřejné IP adresy front-endu fond IP adres.</span><span class="sxs-lookup"><span data-stu-id="d1faf-148">Create a public IP address the front-end IP pool.</span></span>

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="d1faf-149">Nástroj pro vyrovnávání zatížení používá název domény veřejné IP adresy jako svůj plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="d1faf-149">The load balancer uses the domain label of the public IP as its FQDN.</span></span> <span data-ttu-id="d1faf-150">Tato změna z klasického nasazení, který používá cloudové služby název jako nástroj pro vyrovnávání zatížení plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="d1faf-150">This a change from classic deployment, which uses the cloud service name as the load balancer FQDN.</span></span>
    > <span data-ttu-id="d1faf-151">V tomto příkladu je plně kvalifikovaný název domény *contoso09152016.southcentralus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="d1faf-151">In this example, the FQDN is *contoso09152016.southcentralus.cloudapp.azure.com*.</span></span>

## <a name="create-front-end-and-back-end-pools"></a><span data-ttu-id="d1faf-152">Vytvořte front-end a back endové fondy</span><span class="sxs-lookup"><span data-stu-id="d1faf-152">Create front-end and back-end pools</span></span>

<span data-ttu-id="d1faf-153">Tento příklad vytvoří fondu front-end IP, která přijímá příchozí síťový provoz na nástroje pro vyrovnávání zatížení a fond back-end IP, kde front-end fondu odešle síťový provoz s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="d1faf-153">This example creates the front-end IP pool that receives the incoming network traffic on the load balancer and the back-end IP pool where the front-end pool sends the load balanced network traffic.</span></span>

1. <span data-ttu-id="d1faf-154">Nastavení proměnných prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1faf-154">Set up the PowerShell variables</span></span>

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. <span data-ttu-id="d1faf-155">Vytvořte front-endový fond IP adres přidružující veřejnou IP adresu vytvořenou v předchozím kroku k nástroji pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d1faf-155">Create a front-end IP pool associating the public IP created in the previous step and the load balancer.</span></span>

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-the-probe-nat-rules-and-lb-rules"></a><span data-ttu-id="d1faf-156">Vytvoření testu, pravidla NAT a pravidel LB</span><span class="sxs-lookup"><span data-stu-id="d1faf-156">Create the probe, NAT rules, and LB rules</span></span>

<span data-ttu-id="d1faf-157">Tento příklad vytvoří následující položky:</span><span class="sxs-lookup"><span data-stu-id="d1faf-157">This example creates the following items:</span></span>

* <span data-ttu-id="d1faf-158">pravidlo testu zkontrolujte připojení k portům TCP 80 k</span><span class="sxs-lookup"><span data-stu-id="d1faf-158">a probe rule to check for connectivity to TCP port 80</span></span>
* <span data-ttu-id="d1faf-159">pravidlo NAT přeložit všechny příchozí přenosy na portu 3389 k portu 3389 pro protokol RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="d1faf-159">a NAT rule to translate all incoming traffic on port 3389 to port 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="d1faf-160">pravidlo NAT přeložit všechny příchozí přenosy na portu 3391 k portu 3389 pro protokol RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="d1faf-160">a NAT rule to translate all incoming traffic on port 3391 to port 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="d1faf-161">Pravidlo nástroje pro vyrovnávání zatížení, které vyrovnává zatížení veškerého příchozího provozu na portu 80 na port 80 na adresách v back-endovém fondu.</span><span class="sxs-lookup"><span data-stu-id="d1faf-161">a load balancer rule to balance all incoming traffic on port 80 to port 80 on the addresses in the back-end pool.</span></span>

<span data-ttu-id="d1faf-162"><sup>1</sup> Pravidla překladu adres (NAT) se přidružují ke konkrétní instanci virtuálního počítače za nástrojem pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d1faf-162"><sup>1</sup> NAT rules are associated to a specific virtual machine instance behind the load balancer.</span></span> <span data-ttu-id="d1faf-163">Síťový provoz na portu 3389 přicházejících posílá konkrétní virtuální počítač a port pravidla NAT přidružený.</span><span class="sxs-lookup"><span data-stu-id="d1faf-163">The network traffic arriving on port 3389 is sent to the specific virtual machine and port associated with the NAT rule.</span></span> <span data-ttu-id="d1faf-164">Pro pravidlo překladu adres (NAT) je nutné zadat protokol (UDP nebo TCP).</span><span class="sxs-lookup"><span data-stu-id="d1faf-164">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="d1faf-165">Jednomu portu nelze přiřadit oba protokoly.</span><span class="sxs-lookup"><span data-stu-id="d1faf-165">Both protocols can't be assigned to the same port.</span></span>

1. <span data-ttu-id="d1faf-166">Nastavení proměnných prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1faf-166">Set up the PowerShell variables</span></span>

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. <span data-ttu-id="d1faf-167">Vytvoření sonda</span><span class="sxs-lookup"><span data-stu-id="d1faf-167">Create the probe</span></span>

    <span data-ttu-id="d1faf-168">Následující příklad vytvoří sondou TCP, který kontroluje pro připojení k back-end port TCP 80 každých 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="d1faf-168">The following example creates a TCP probe that checks for connectivity to back-end TCP port 80 every 15 seconds.</span></span> <span data-ttu-id="d1faf-169">Ho označíte back-end prostředku není k dispozici po dvě po sobě jdoucích selhání.</span><span class="sxs-lookup"><span data-stu-id="d1faf-169">It will mark the back-end resource unavailable after two consecutive failures.</span></span>

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. <span data-ttu-id="d1faf-170">Vytvoření příchozích pravidel NAT, které umožňují připojení RDP k prostředkům back-end</span><span class="sxs-lookup"><span data-stu-id="d1faf-170">Create inbound NAT rules that allow RDP connections to the back-end resources</span></span>

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. <span data-ttu-id="d1faf-171">Vytvořit pravidla, která odesílá data na jiné porty back-end v závislosti na které front-endu přijala požadavek na službu Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d1faf-171">Create a load balancer rules that send traffic to different back-end ports depending on which front end received the request</span></span>

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. <span data-ttu-id="d1faf-172">Zkontrolujte nastavení</span><span class="sxs-lookup"><span data-stu-id="d1faf-172">Check your settings</span></span>

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    <span data-ttu-id="d1faf-173">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d1faf-173">Expected output:</span></span>

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
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

## <a name="create-nics"></a><span data-ttu-id="d1faf-174">Vytvoření síťových rozhraní</span><span class="sxs-lookup"><span data-stu-id="d1faf-174">Create NICs</span></span>

<span data-ttu-id="d1faf-175">Vytvoření síťové adaptéry a přidružovat je k pravidla NAT, pravidla nástroje pro vyrovnávání zatížení a sondy.</span><span class="sxs-lookup"><span data-stu-id="d1faf-175">Create NICs and associate them to NAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="d1faf-176">Nastavení proměnných prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1faf-176">Set up the PowerShell variables</span></span>

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

2. <span data-ttu-id="d1faf-177">Vytvořte síťovou kartu pro každý back-end a přidejte konfigurace protokolu IPv6.</span><span class="sxs-lookup"><span data-stu-id="d1faf-177">Create a NIC for each back-end and add an IPv6 configuration.</span></span>

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a><span data-ttu-id="d1faf-178">Vytvořte prostředky virtuálních počítačů back-end a připojte každý síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="d1faf-178">Create the back-end VM resources and attach each NIC</span></span>

<span data-ttu-id="d1faf-179">Pokud chcete vytvořit virtuální počítače, musí mít účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1faf-179">To create VMs, you must have a storage account.</span></span> <span data-ttu-id="d1faf-180">Pro vyrovnávání zatížení, musí být členy skupiny dostupnosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d1faf-180">For load balancing, the VMs need to be members of an availability set.</span></span> <span data-ttu-id="d1faf-181">Další informace o vytváření virtuálních počítačů najdete v tématu [vytvoření virtuálního počítače Azure pomocí prostředí PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d1faf-181">For more information about creating VMs, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="d1faf-182">Nastavení proměnných prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d1faf-182">Set up the PowerShell variables</span></span>

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
    > <span data-ttu-id="d1faf-183">Tento příklad používá uživatelské jméno a heslo pro virtuální počítače ve formátu prostého textu.</span><span class="sxs-lookup"><span data-stu-id="d1faf-183">This example uses the username and password for the VMs in clear text.</span></span> <span data-ttu-id="d1faf-184">Odpovídající musí dát pozor při použití přihlašovacích údajů v nešifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="d1faf-184">Appropriate care must be taken when using credentials in the clear.</span></span> <span data-ttu-id="d1faf-185">Bezpečnější způsob zpracování pověření v prostředí PowerShell, najdete v článku [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="d1faf-185">For a more secure method of handling credentials in PowerShell, see the [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

2. <span data-ttu-id="d1faf-186">Vytvořit sadu dostupnosti a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="d1faf-186">Create the storage account and availability set</span></span>

    <span data-ttu-id="d1faf-187">Při vytváření virtuálních počítačů, může použít existující účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1faf-187">You may use an existing storage account when you create the VMs.</span></span> <span data-ttu-id="d1faf-188">Následující příkaz vytvoří nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d1faf-188">The following command creates a new storage account.</span></span>

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

    <span data-ttu-id="d1faf-189">Dále vytvořte skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="d1faf-189">Next, create the availability set.</span></span>

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. <span data-ttu-id="d1faf-190">Vytvoření virtuálních počítačů s přidružené síťové karty</span><span class="sxs-lookup"><span data-stu-id="d1faf-190">Create the virtual machines with the associated NICs</span></span>

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a><span data-ttu-id="d1faf-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d1faf-191">Next steps</span></span>

[<span data-ttu-id="d1faf-192">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d1faf-192">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="d1faf-193">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d1faf-193">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="d1faf-194">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d1faf-194">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
