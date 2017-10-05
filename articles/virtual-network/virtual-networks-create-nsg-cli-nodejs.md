---
title: "Vytvoření skupin zabezpečení sítě - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak vytvořit a nasadit pomocí Azure CLI 1.0 skupin zabezpečení sítě."
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
ms.openlocfilehash: ca8c182651e3c9f2f1f3a85b94361755d8e638d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-the-azure-cli-10"></a><span data-ttu-id="65616-103">Vytvořit síť pomocí Azure CLI 1.0 skupin zabezpečení</span><span class="sxs-lookup"><span data-stu-id="65616-103">Create network security groups using the Azure CLI 1.0</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="65616-104">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="65616-104">CLI versions to complete the task</span></span> 

<span data-ttu-id="65616-105">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="65616-105">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="65616-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="65616-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="65616-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) -naše generace CLI pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="65616-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) - our next-generation CLI for the resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="65616-108">Tento článek se týká modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="65616-108">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="65616-109">Můžete také [vytvářet skupiny Nsg v modelu nasazení classic](virtual-networks-create-nsg-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="65616-109">You can also [create NSGs in the classic deployment model](virtual-networks-create-nsg-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="65616-110">Níže uvedené příkazy rozhraní příkazového řádku Azure ukázka očekávat jednoduché prostředí již vytvořeny podle výše uvedené scénáře.</span><span class="sxs-lookup"><span data-stu-id="65616-110">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> 

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a><span data-ttu-id="65616-111">Postup vytvoření skupina NSG pro podsítě front end</span><span class="sxs-lookup"><span data-stu-id="65616-111">How to create the NSG for the front end subnet</span></span>
<span data-ttu-id="65616-112">Chcete-li vytvořit skupinu NSG s názvem s názvem *NSG front-endu* závislosti na scénáři výše, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="65616-112">To create an NSG named named *NSG-FrontEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="65616-113">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="65616-113">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="65616-114">Spuštěním příkazu **azure config mode** přejděte do režimu Resource Manager, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="65616-114">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="65616-115">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="65616-115">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="65616-116">Spustit **nsg síť azure vytvořit** příkazu vytvořte skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="65616-116">Run the **azure network nsg create** command to create an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd
   
    <span data-ttu-id="65616-117">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="65616-117">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="65616-118">Parametry:</span><span class="sxs-lookup"><span data-stu-id="65616-118">Parameters:</span></span>
   
   * <span data-ttu-id="65616-119">**-g (nebo --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="65616-119">**-g (or --resource-group)**.</span></span> <span data-ttu-id="65616-120">Název skupiny prostředků, kde bude vytvořen NSG.</span><span class="sxs-lookup"><span data-stu-id="65616-120">Name of the resource group where the NSG will be created.</span></span> <span data-ttu-id="65616-121">V našem scénáři je to *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="65616-121">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="65616-122">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="65616-122">**-l (or --location)**.</span></span> <span data-ttu-id="65616-123">Oblast Azure, kde bude vytvořena nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="65616-123">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="65616-124">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="65616-124">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="65616-125">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="65616-125">**-n (or --name)**.</span></span> <span data-ttu-id="65616-126">Název nové skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="65616-126">Name for the new NSG.</span></span> <span data-ttu-id="65616-127">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="65616-127">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="65616-128">Spustit **vytvořit pravidla nsg síť azure** příkaz k vytvoření pravidla, která umožňuje přístup k portu 3389 (RDP) z Internetu.</span><span class="sxs-lookup"><span data-stu-id="65616-128">Run the **azure network nsg rule create** command to create a rule that allows access to port 3389 (RDP) from the Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="65616-129">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="65616-129">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up the network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="65616-130">Parametry:</span><span class="sxs-lookup"><span data-stu-id="65616-130">Parameters:</span></span>
   
   * <span data-ttu-id="65616-131">**-a (nebo--nsg-name)**.</span><span class="sxs-lookup"><span data-stu-id="65616-131">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="65616-132">Název skupiny NSG, ve kterém se vytvoří pravidlo.</span><span class="sxs-lookup"><span data-stu-id="65616-132">Name of the NSG in which the rule will be created.</span></span> <span data-ttu-id="65616-133">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="65616-133">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="65616-134">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="65616-134">**-n (or --name)**.</span></span> <span data-ttu-id="65616-135">Název pro nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="65616-135">Name for the new rule.</span></span> <span data-ttu-id="65616-136">Pro náš scénář *rdp pravidlo*.</span><span class="sxs-lookup"><span data-stu-id="65616-136">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="65616-137">**-c (nebo--přístup)**.</span><span class="sxs-lookup"><span data-stu-id="65616-137">**-c (or --access)**.</span></span> <span data-ttu-id="65616-138">Úroveň přístupu pro pravidlo (zakázat nebo povolit).</span><span class="sxs-lookup"><span data-stu-id="65616-138">Access level for the rule (Deny or Allow).</span></span>
   * <span data-ttu-id="65616-139">**-p (nebo--protocol)**.</span><span class="sxs-lookup"><span data-stu-id="65616-139">**-p (or --protocol)**.</span></span> <span data-ttu-id="65616-140">Protokol (Tcp, Udp nebo *) pro pravidlo.</span><span class="sxs-lookup"><span data-stu-id="65616-140">Protocol (Tcp, Udp, or *) for the rule.</span></span>
   * <span data-ttu-id="65616-141">**-r (nebo--směr)**.</span><span class="sxs-lookup"><span data-stu-id="65616-141">**-r (or --direction)**.</span></span> <span data-ttu-id="65616-142">Směr připojení (příchozí nebo odchozí).</span><span class="sxs-lookup"><span data-stu-id="65616-142">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="65616-143">**-y (nebo--priority)**.</span><span class="sxs-lookup"><span data-stu-id="65616-143">**-y (or --priority)**.</span></span> <span data-ttu-id="65616-144">Priorita pravidla.</span><span class="sxs-lookup"><span data-stu-id="65616-144">Priority for the rule.</span></span>
   * <span data-ttu-id="65616-145">**-f (nebo--předpona zdrojové adresy)**.</span><span class="sxs-lookup"><span data-stu-id="65616-145">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="65616-146">Předpona zdrojové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="65616-146">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="65616-147">**-e (nebo--rozsah zdrojových portů)**.</span><span class="sxs-lookup"><span data-stu-id="65616-147">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="65616-148">Zdrojový port, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="65616-148">Source port, or port range.</span></span>
   * <span data-ttu-id="65616-149">**-e (nebo--předpona cílové adresy)**.</span><span class="sxs-lookup"><span data-stu-id="65616-149">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="65616-150">Předpona cílové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="65616-150">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="65616-151">**-u (nebo--rozsah cílových portů)**.</span><span class="sxs-lookup"><span data-stu-id="65616-151">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="65616-152">Cílový port, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="65616-152">Destination port, or port range.</span></span>    
5. <span data-ttu-id="65616-153">Spustit **vytvořit pravidla nsg síť azure** příkaz k vytvoření pravidla, která umožňuje přístup k portu 80 (HTTP) z Internetu.</span><span class="sxs-lookup"><span data-stu-id="65616-153">Run the **azure network nsg rule create** command to create a rule that allows access to port 80 (HTTP) from the Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="65616-154">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="65616-154">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
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
6. <span data-ttu-id="65616-155">Spustit **sítě azure vnet podsíť sadu** příkaz propojení NSG na podsítě front end.</span><span class="sxs-lookup"><span data-stu-id="65616-155">Run the **azure network vnet subnet set** command to link the NSG to the front end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd
   
    <span data-ttu-id="65616-156">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="65616-156">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
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

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a><span data-ttu-id="65616-157">Postup vytvoření skupina NSG pro podsíť back-end</span><span class="sxs-lookup"><span data-stu-id="65616-157">How to create the NSG for the back end subnet</span></span>
<span data-ttu-id="65616-158">Chcete-li vytvořit skupinu NSG s názvem s názvem *NSG back-end* závislosti na scénáři výše, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="65616-158">To create an NSG named named *NSG-BackEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="65616-159">Spustit **nsg síť azure vytvořit** příkazu vytvořte skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="65616-159">Run the **azure network nsg create** command to create an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-BackEnd
   
    <span data-ttu-id="65616-160">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="65616-160">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
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
2. <span data-ttu-id="65616-161">Spustit **vytvořit pravidla nsg síť azure** příkaz k vytvoření pravidla, která umožňuje přístup k portu 1433 (SQL) z podsítě front end.</span><span class="sxs-lookup"><span data-stu-id="65616-161">Run the **azure network nsg rule create** command to create a rule that allows access to port 1433 (SQL) from the front end subnet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="65616-162">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="65616-162">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
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
3. <span data-ttu-id="65616-163">Spustit **vytvořit pravidla nsg síť azure** příkaz pro vytvoření pravidla, které odepře přístup k Internetu z.</span><span class="sxs-lookup"><span data-stu-id="65616-163">Run the **azure network nsg rule create** command to create a rule that denies access to the Internet from.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *
   
    <span data-ttu-id="65616-164">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="65616-164">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
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
4. <span data-ttu-id="65616-165">Spustit **sítě azure vnet podsíť sadu** příkaz propojení NSG k podsíti back-end.</span><span class="sxs-lookup"><span data-stu-id="65616-165">Run the **azure network vnet subnet set** command to link the NSG to the back end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd
   
    <span data-ttu-id="65616-166">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="65616-166">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up the subnet "BackEnd"
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

