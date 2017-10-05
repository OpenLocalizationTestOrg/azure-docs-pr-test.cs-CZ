---
title: "Postup vytvoření skupin Nsg v klasickém režimu pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Postup vytvoření a nasazení skupin Nsg v klasickém režimu pomocí rozhraní příkazového řádku Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 115a1937a4c88ba2b986a40c84b1b759ed5e03b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a><span data-ttu-id="44200-103">Vytvoření skupiny zabezpečení sítě (klasické) v rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="44200-103">How to create NSGs (classic) in the Azure CLI</span></span>
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="44200-104">Tento článek se týká modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="44200-104">This article covers the classic deployment model.</span></span> <span data-ttu-id="44200-105">Můžete také [vytvářet skupiny Nsg v modelu nasazení Resource Manager](virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="44200-105">You can also [create NSGs in the Resource Manager deployment model](virtual-networks-create-nsg-arm-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="44200-106">Níže uvedené příkazy rozhraní příkazového řádku Azure ukázka očekávat jednoduché prostředí již vytvořeny podle výše uvedené scénáře.</span><span class="sxs-lookup"><span data-stu-id="44200-106">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="44200-107">Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovací prostředí podle [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="44200-107">If you want to run the commands as they are displayed in this document, first build the test environment by [creating a VNet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a><span data-ttu-id="44200-108">Postup vytvoření skupina NSG pro podsítě front end</span><span class="sxs-lookup"><span data-stu-id="44200-108">How to create the NSG for the front end subnet</span></span>
<span data-ttu-id="44200-109">Chcete-li vytvořit skupinu NSG s názvem s názvem **NSG front-endu** závislosti na scénáři výše, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="44200-109">To create an NSG named named **NSG-FrontEnd** based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="44200-110">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="44200-110">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="44200-111">Spustit  **`azure config mode`**  příkaz Přepnout do klasického režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="44200-111">Run the **`azure config mode`** command to switch to classic mode, as shown below.</span></span>
   
        azure config mode asm
   
    <span data-ttu-id="44200-112">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="44200-112">Expected output:</span></span>
   
        info:    New mode is asm
3. <span data-ttu-id="44200-113">Spustit  **`azure network nsg create`**  příkazu vytvořte skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="44200-113">Run the **`azure network nsg create`** command to create an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    <span data-ttu-id="44200-114">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="44200-114">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    <span data-ttu-id="44200-115">Parametry:</span><span class="sxs-lookup"><span data-stu-id="44200-115">Parameters:</span></span>
   
   * <span data-ttu-id="44200-116">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="44200-116">**-l (or --location)**.</span></span> <span data-ttu-id="44200-117">Oblast Azure, kde bude vytvořena nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="44200-117">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="44200-118">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="44200-118">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="44200-119">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="44200-119">**-n (or --name)**.</span></span> <span data-ttu-id="44200-120">Název nové skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="44200-120">Name for the new NSG.</span></span> <span data-ttu-id="44200-121">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="44200-121">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="44200-122">Spustit  **`azure network nsg rule create`**  příkaz k vytvoření pravidla, která umožňuje přístup k portu 3389 (RDP) z Internetu.</span><span class="sxs-lookup"><span data-stu-id="44200-122">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 3389 (RDP) from the Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="44200-123">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="44200-123">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    <span data-ttu-id="44200-124">Parametry:</span><span class="sxs-lookup"><span data-stu-id="44200-124">Parameters:</span></span>
   
   * <span data-ttu-id="44200-125">**-a (nebo--nsg-name)**.</span><span class="sxs-lookup"><span data-stu-id="44200-125">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="44200-126">Název skupiny NSG, ve kterém se vytvoří pravidlo.</span><span class="sxs-lookup"><span data-stu-id="44200-126">Name of the NSG in which the rule will be created.</span></span> <span data-ttu-id="44200-127">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="44200-127">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="44200-128">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="44200-128">**-n (or --name)**.</span></span> <span data-ttu-id="44200-129">Název pro nové pravidlo.</span><span class="sxs-lookup"><span data-stu-id="44200-129">Name for the new rule.</span></span> <span data-ttu-id="44200-130">Pro náš scénář *rdp pravidlo*.</span><span class="sxs-lookup"><span data-stu-id="44200-130">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="44200-131">**-c (nebo--akce)**.</span><span class="sxs-lookup"><span data-stu-id="44200-131">**-c (or --action)**.</span></span> <span data-ttu-id="44200-132">Úroveň přístupu pro pravidlo (zakázat nebo povolit).</span><span class="sxs-lookup"><span data-stu-id="44200-132">Access level for the rule (Deny or Allow).</span></span>
   * <span data-ttu-id="44200-133">**-p (nebo--protocol)**.</span><span class="sxs-lookup"><span data-stu-id="44200-133">**-p (or --protocol)**.</span></span> <span data-ttu-id="44200-134">Protokol (Tcp, Udp nebo *) pro pravidlo.</span><span class="sxs-lookup"><span data-stu-id="44200-134">Protocol (Tcp, Udp, or *) for the rule.</span></span>
   * <span data-ttu-id="44200-135">**-r (nebo--typu)**.</span><span class="sxs-lookup"><span data-stu-id="44200-135">**-r (or --type)**.</span></span> <span data-ttu-id="44200-136">Směr připojení (příchozí nebo odchozí).</span><span class="sxs-lookup"><span data-stu-id="44200-136">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="44200-137">**-y (nebo--priority)**.</span><span class="sxs-lookup"><span data-stu-id="44200-137">**-y (or --priority)**.</span></span> <span data-ttu-id="44200-138">Priorita pravidla.</span><span class="sxs-lookup"><span data-stu-id="44200-138">Priority for the rule.</span></span>
   * <span data-ttu-id="44200-139">**-f (nebo--předpona zdrojové adresy)**.</span><span class="sxs-lookup"><span data-stu-id="44200-139">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="44200-140">Předpona zdrojové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="44200-140">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="44200-141">**-e (nebo--rozsah zdrojových portů)**.</span><span class="sxs-lookup"><span data-stu-id="44200-141">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="44200-142">Zdrojový port, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="44200-142">Source port, or port range.</span></span>
   * <span data-ttu-id="44200-143">**-e (nebo--předpona cílové adresy)**.</span><span class="sxs-lookup"><span data-stu-id="44200-143">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="44200-144">Předpona cílové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="44200-144">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="44200-145">**-u (nebo--rozsah cílových portů)**.</span><span class="sxs-lookup"><span data-stu-id="44200-145">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="44200-146">Cílový port, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="44200-146">Destination port, or port range.</span></span>
5. <span data-ttu-id="44200-147">Spustit  **`azure network nsg rule create`**  příkaz k vytvoření pravidla, která umožňuje přístup k portu 80 (HTTP) z Internetu.</span><span class="sxs-lookup"><span data-stu-id="44200-147">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 80 (HTTP) from the Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="44200-148">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="44200-148">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. <span data-ttu-id="44200-149">Spustit  **`azure network nsg subnet add`**  příkaz propojení NSG na podsítě front end.</span><span class="sxs-lookup"><span data-stu-id="44200-149">Run the **`azure network nsg subnet add`** command to link the NSG to the front end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    <span data-ttu-id="44200-150">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="44200-150">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a><span data-ttu-id="44200-151">Postup vytvoření skupina NSG pro podsíť back-end</span><span class="sxs-lookup"><span data-stu-id="44200-151">How to create the NSG for the back end subnet</span></span>
<span data-ttu-id="44200-152">Chcete-li vytvořit skupinu NSG s názvem s názvem *NSG back-end* závislosti na scénáři výše, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="44200-152">To create an NSG named named *NSG-BackEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="44200-153">Spustit  **`azure network nsg create`**  příkazu vytvořte skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="44200-153">Run the **`azure network nsg create`** command to create an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    <span data-ttu-id="44200-154">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="44200-154">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    <span data-ttu-id="44200-155">Parametry:</span><span class="sxs-lookup"><span data-stu-id="44200-155">Parameters:</span></span>
   
   * <span data-ttu-id="44200-156">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="44200-156">**-l (or --location)**.</span></span> <span data-ttu-id="44200-157">Oblast Azure, kde bude vytvořena nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="44200-157">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="44200-158">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="44200-158">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="44200-159">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="44200-159">**-n (or --name)**.</span></span> <span data-ttu-id="44200-160">Název nové skupiny NSG.</span><span class="sxs-lookup"><span data-stu-id="44200-160">Name for the new NSG.</span></span> <span data-ttu-id="44200-161">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="44200-161">For our scenario, *NSG-FrontEnd*.</span></span>
2. <span data-ttu-id="44200-162">Spustit  **`azure network nsg rule create`**  příkaz k vytvoření pravidla, která umožňuje přístup k portu 1433 (SQL) z podsítě front end.</span><span class="sxs-lookup"><span data-stu-id="44200-162">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 1433 (SQL) from the front end subnet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="44200-163">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="44200-163">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. <span data-ttu-id="44200-164">Spustit  **`azure network nsg rule create`**  příkaz pro vytvoření pravidla, které odepře přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="44200-164">Run the **`azure network nsg rule create`** command to create a rule that denies access to the Internet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    <span data-ttu-id="44200-165">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="44200-165">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. <span data-ttu-id="44200-166">Spustit  **`azure network nsg subnet add`**  příkaz propojení NSG k podsíti back-end.</span><span class="sxs-lookup"><span data-stu-id="44200-166">Run the **`azure network nsg subnet add`** command to link the NSG to the back end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    <span data-ttu-id="44200-167">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="44200-167">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

