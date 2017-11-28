---
title: "aaaCreate skupin zabezpečení - sítě, Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení skupin zabezpečení sítě pomocí hello Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eeb7feedab959d92659e03c5c46d93fdfc08faea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-cli-10"></a><span data-ttu-id="d6055-103">Vytvořit síť pomocí Azure CLI 1.0 hello skupin zabezpečení</span><span class="sxs-lookup"><span data-stu-id="d6055-103">Create network security groups using hello Azure CLI 1.0</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="d6055-104">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="d6055-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="d6055-105">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="d6055-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="d6055-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="d6055-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="d6055-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) -naše generace CLI pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="d6055-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) - our next-generation CLI for hello resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="d6055-108">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="d6055-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="d6055-109">Můžete také [vytvářet skupiny Nsg v modelu nasazení classic hello](virtual-networks-create-nsg-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="d6055-109">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="d6055-110">níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello výše.</span><span class="sxs-lookup"><span data-stu-id="d6055-110">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> 

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="d6055-111">Jak toocreate hello skupina NSG pro podsítě front end hello</span><span class="sxs-lookup"><span data-stu-id="d6055-111">How toocreate hello NSG for hello front end subnet</span></span>
<span data-ttu-id="d6055-112">toocreate s názvem skupiny NSG s názvem *NSG front-endu* podle hello scénář výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="d6055-112">toocreate an NSG named named *NSG-FrontEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="d6055-113">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="d6055-113">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="d6055-114">Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="d6055-114">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="d6055-115">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d6055-115">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="d6055-116">Spustit hello **vytvořit síť azure nsg** příkaz toocreate skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="d6055-116">Run hello **azure network nsg create** command toocreate an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd
   
    <span data-ttu-id="d6055-117">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d6055-117">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
        data:    Name                            : NSG-FrontEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK
   
    <span data-ttu-id="d6055-118">Parametry:</span><span class="sxs-lookup"><span data-stu-id="d6055-118">Parameters:</span></span>
   
   * <span data-ttu-id="d6055-119">**-g (nebo --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-119">**-g (or --resource-group)**.</span></span> <span data-ttu-id="d6055-120">Název skupiny prostředků hello, kde bude vytvořen hello NSG.</span><span class="sxs-lookup"><span data-stu-id="d6055-120">Name of hello resource group where hello NSG will be created.</span></span> <span data-ttu-id="d6055-121">V našem scénáři je to *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="d6055-121">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="d6055-122">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-122">**-l (or --location)**.</span></span> <span data-ttu-id="d6055-123">Oblast Azure, kde hello nová skupina NSG bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="d6055-123">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="d6055-124">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="d6055-124">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="d6055-125">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-125">**-n (or --name)**.</span></span> <span data-ttu-id="d6055-126">Název hello nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="d6055-126">Name for hello new NSG.</span></span> <span data-ttu-id="d6055-127">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="d6055-127">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="d6055-128">Spustit hello **vytvořit pravidla nsg síť azure** příkaz toocreate pravidlo, které umožní přístup tooport 3389 (RDP) z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="d6055-128">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 3389 (RDP) from hello Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="d6055-129">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d6055-129">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up hello network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp
        -rule
        data:    Name                            : rdp-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 3389
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    <span data-ttu-id="d6055-130">Parametry:</span><span class="sxs-lookup"><span data-stu-id="d6055-130">Parameters:</span></span>
   
   * <span data-ttu-id="d6055-131">**-a (nebo--nsg-name)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-131">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="d6055-132">Název hello NSG, ve které hello se vytvoří pravidlo.</span><span class="sxs-lookup"><span data-stu-id="d6055-132">Name of hello NSG in which hello rule will be created.</span></span> <span data-ttu-id="d6055-133">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="d6055-133">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="d6055-134">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-134">**-n (or --name)**.</span></span> <span data-ttu-id="d6055-135">Název pro nové pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="d6055-135">Name for hello new rule.</span></span> <span data-ttu-id="d6055-136">Pro náš scénář *rdp pravidlo*.</span><span class="sxs-lookup"><span data-stu-id="d6055-136">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="d6055-137">**-c (nebo--přístup)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-137">**-c (or --access)**.</span></span> <span data-ttu-id="d6055-138">Úroveň přístupu pro pravidlo hello (zakázat nebo povolit).</span><span class="sxs-lookup"><span data-stu-id="d6055-138">Access level for hello rule (Deny or Allow).</span></span>
   * <span data-ttu-id="d6055-139">**-p (nebo--protocol)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-139">**-p (or --protocol)**.</span></span> <span data-ttu-id="d6055-140">Protokol (Tcp, Udp nebo *) pro pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="d6055-140">Protocol (Tcp, Udp, or *) for hello rule.</span></span>
   * <span data-ttu-id="d6055-141">**-r (nebo--směr)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-141">**-r (or --direction)**.</span></span> <span data-ttu-id="d6055-142">Směr připojení (příchozí nebo odchozí).</span><span class="sxs-lookup"><span data-stu-id="d6055-142">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="d6055-143">**-y (nebo--priority)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-143">**-y (or --priority)**.</span></span> <span data-ttu-id="d6055-144">Priorita pravidla hello.</span><span class="sxs-lookup"><span data-stu-id="d6055-144">Priority for hello rule.</span></span>
   * <span data-ttu-id="d6055-145">**-f (nebo--předpona zdrojové adresy)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-145">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="d6055-146">Předpona zdrojové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="d6055-146">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="d6055-147">**-e (nebo--rozsah zdrojových portů)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-147">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="d6055-148">Zdrojový port, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="d6055-148">Source port, or port range.</span></span>
   * <span data-ttu-id="d6055-149">**-e (nebo--předpona cílové adresy)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-149">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="d6055-150">Předpona cílové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="d6055-150">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="d6055-151">**-u (nebo--rozsah cílových portů)**.</span><span class="sxs-lookup"><span data-stu-id="d6055-151">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="d6055-152">Cílový port, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="d6055-152">Destination port, or port range.</span></span>    
5. <span data-ttu-id="d6055-153">Spustit hello **vytvořit pravidla nsg síť azure** příkaz toocreate pravidlo, které umožní přístup tooport 80 (HTTP) z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="d6055-153">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 80 (HTTP) from hello Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="d6055-154">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="d6055-154">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 80
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. <span data-ttu-id="d6055-155">Spustit hello **sítě azure vnet podsíť sadu** příkaz toolink hello NSG toohello podsítě front end.</span><span class="sxs-lookup"><span data-stu-id="d6055-155">Run hello **azure network vnet subnet set** command toolink hello NSG toohello front end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd
   
    <span data-ttu-id="d6055-156">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d6055-156">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="d6055-157">Jak toocreate hello skupina NSG pro hello zpět ukončení podsítě</span><span class="sxs-lookup"><span data-stu-id="d6055-157">How toocreate hello NSG for hello back end subnet</span></span>
<span data-ttu-id="d6055-158">toocreate s názvem skupiny NSG s názvem *NSG back-end* podle hello scénář výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="d6055-158">toocreate an NSG named named *NSG-BackEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="d6055-159">Spustit hello **vytvořit síť azure nsg** příkaz toocreate skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="d6055-159">Run hello **azure network nsg create** command toocreate an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-BackEnd
   
    <span data-ttu-id="d6055-160">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d6055-160">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd
        data:    Name                            : NSG-BackEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK
2. <span data-ttu-id="d6055-161">Spustit hello **vytvořit pravidla nsg síť azure** příkaz toocreate pravidlo, které umožní přístup tooport 1433 (SQL) z podsítě front end hello.</span><span class="sxs-lookup"><span data-stu-id="d6055-161">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 1433 (SQL) from hello front end subnet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="d6055-162">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d6055-162">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule
        data:    Name                            : sql-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 1433
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. <span data-ttu-id="d6055-163">Spustit hello **vytvořit pravidla nsg síť azure** příkaz toocreate pravidlo, které zakazuje toohello přístup k Internetu z.</span><span class="sxs-lookup"><span data-stu-id="d6055-163">Run hello **azure network nsg rule create** command toocreate a rule that denies access toohello Internet from.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *
   
    <span data-ttu-id="d6055-164">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="d6055-164">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : *
        data:    Source Port                     : *
        data:    Destination IP                  : Internet
        data:    Destination Port                : *
        data:    Protocol                        : *
        data:    Direction                       : Outbound
        data:    Access                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. <span data-ttu-id="d6055-165">Spustit hello **sítě azure vnet podsíť sadu** příkaz toolink hello NSG toohello zpět ukončení podsítě.</span><span class="sxs-lookup"><span data-stu-id="d6055-165">Run hello **azure network vnet subnet set** command toolink hello NSG toohello back end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd
   
    <span data-ttu-id="d6055-166">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d6055-166">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up hello subnet "BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/BackEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : BackEnd
        data:    Address prefix                  : 192.168.2.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL1/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL2/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

