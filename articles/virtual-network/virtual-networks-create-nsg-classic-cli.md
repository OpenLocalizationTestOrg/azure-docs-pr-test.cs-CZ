---
title: "hello aaaHow toocreate skupin Nsg v klasickém režimu pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení skupin Nsg v klasickém režimu pomocí hello rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: eb78861e10a0dd950bb2c3783ee957d1cce55016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-nsgs-classic-in-hello-azure-cli"></a><span data-ttu-id="56fa2-103">Jak toocreate skupiny zabezpečení sítě (klasické) v hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="56fa2-103">How toocreate NSGs (classic) in hello Azure CLI</span></span>
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="56fa2-104">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="56fa2-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="56fa2-105">Můžete také [vytvářet skupiny Nsg v modelu nasazení Resource Manager hello](virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="56fa2-105">You can also [create NSGs in hello Resource Manager deployment model](virtual-networks-create-nsg-arm-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="56fa2-106">níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello výše.</span><span class="sxs-lookup"><span data-stu-id="56fa2-106">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="56fa2-107">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí podle [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="56fa2-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment by [creating a VNet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="56fa2-108">Jak toocreate hello skupina NSG pro podsítě front end hello</span><span class="sxs-lookup"><span data-stu-id="56fa2-108">How toocreate hello NSG for hello front end subnet</span></span>
<span data-ttu-id="56fa2-109">toocreate s názvem skupiny NSG s názvem **NSG front-endu** podle hello scénář výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="56fa2-109">toocreate an NSG named named **NSG-FrontEnd** based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="56fa2-110">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="56fa2-110">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="56fa2-111">Spustit hello  **`azure config mode`**  příkaz tooswitch tooclassic režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="56fa2-111">Run hello **`azure config mode`** command tooswitch tooclassic mode, as shown below.</span></span>
   
        azure config mode asm
   
    <span data-ttu-id="56fa2-112">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56fa2-112">Expected output:</span></span>
   
        info:    New mode is asm
3. <span data-ttu-id="56fa2-113">Spustit hello  **`azure network nsg create`**  příkaz toocreate skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="56fa2-113">Run hello **`azure network nsg create`** command toocreate an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    <span data-ttu-id="56fa2-114">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56fa2-114">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="56fa2-115">Parametry:</span><span class="sxs-lookup"><span data-stu-id="56fa2-115">Parameters:</span></span>
   
   * <span data-ttu-id="56fa2-116">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-116">**-l (or --location)**.</span></span> <span data-ttu-id="56fa2-117">Oblast Azure, kde hello nová skupina NSG bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="56fa2-117">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="56fa2-118">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="56fa2-118">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="56fa2-119">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-119">**-n (or --name)**.</span></span> <span data-ttu-id="56fa2-120">Název hello nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="56fa2-120">Name for hello new NSG.</span></span> <span data-ttu-id="56fa2-121">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="56fa2-121">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="56fa2-122">Spustit hello  **`azure network nsg rule create`**  příkaz toocreate pravidlo, které umožní přístup tooport 3389 (RDP) z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="56fa2-122">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 3389 (RDP) from hello Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="56fa2-123">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56fa2-123">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="56fa2-124">Parametry:</span><span class="sxs-lookup"><span data-stu-id="56fa2-124">Parameters:</span></span>
   
   * <span data-ttu-id="56fa2-125">**-a (nebo--nsg-name)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-125">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="56fa2-126">Název hello NSG, ve které hello se vytvoří pravidlo.</span><span class="sxs-lookup"><span data-stu-id="56fa2-126">Name of hello NSG in which hello rule will be created.</span></span> <span data-ttu-id="56fa2-127">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="56fa2-127">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="56fa2-128">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-128">**-n (or --name)**.</span></span> <span data-ttu-id="56fa2-129">Název pro nové pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="56fa2-129">Name for hello new rule.</span></span> <span data-ttu-id="56fa2-130">Pro náš scénář *rdp pravidlo*.</span><span class="sxs-lookup"><span data-stu-id="56fa2-130">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="56fa2-131">**-c (nebo--akce)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-131">**-c (or --action)**.</span></span> <span data-ttu-id="56fa2-132">Úroveň přístupu pro pravidlo hello (zakázat nebo povolit).</span><span class="sxs-lookup"><span data-stu-id="56fa2-132">Access level for hello rule (Deny or Allow).</span></span>
   * <span data-ttu-id="56fa2-133">**-p (nebo--protocol)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-133">**-p (or --protocol)**.</span></span> <span data-ttu-id="56fa2-134">Protokol (Tcp, Udp nebo *) pro pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="56fa2-134">Protocol (Tcp, Udp, or *) for hello rule.</span></span>
   * <span data-ttu-id="56fa2-135">**-r (nebo--typu)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-135">**-r (or --type)**.</span></span> <span data-ttu-id="56fa2-136">Směr připojení (příchozí nebo odchozí).</span><span class="sxs-lookup"><span data-stu-id="56fa2-136">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="56fa2-137">**-y (nebo--priority)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-137">**-y (or --priority)**.</span></span> <span data-ttu-id="56fa2-138">Priorita pravidla hello.</span><span class="sxs-lookup"><span data-stu-id="56fa2-138">Priority for hello rule.</span></span>
   * <span data-ttu-id="56fa2-139">**-f (nebo--předpona zdrojové adresy)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-139">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="56fa2-140">Předpona zdrojové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="56fa2-140">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="56fa2-141">**-e (nebo--rozsah zdrojových portů)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-141">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="56fa2-142">Zdrojový port, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="56fa2-142">Source port, or port range.</span></span>
   * <span data-ttu-id="56fa2-143">**-e (nebo--předpona cílové adresy)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-143">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="56fa2-144">Předpona cílové adresy v CIDR nebo pomocí výchozí značky.</span><span class="sxs-lookup"><span data-stu-id="56fa2-144">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="56fa2-145">**-u (nebo--rozsah cílových portů)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-145">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="56fa2-146">Cílový port, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="56fa2-146">Destination port, or port range.</span></span>
5. <span data-ttu-id="56fa2-147">Spustit hello  **`azure network nsg rule create`**  příkaz toocreate pravidlo, které umožní přístup tooport 80 (HTTP) z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="56fa2-147">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 80 (HTTP) from hello Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="56fa2-148">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="56fa2-148">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
6. <span data-ttu-id="56fa2-149">Spustit hello  **`azure network nsg subnet add`**  příkaz toolink hello NSG toohello podsítě front end.</span><span class="sxs-lookup"><span data-stu-id="56fa2-149">Run hello **`azure network nsg subnet add`** command toolink hello NSG toohello front end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    <span data-ttu-id="56fa2-150">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56fa2-150">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="56fa2-151">Jak toocreate hello skupina NSG pro hello zpět ukončení podsítě</span><span class="sxs-lookup"><span data-stu-id="56fa2-151">How toocreate hello NSG for hello back end subnet</span></span>
<span data-ttu-id="56fa2-152">toocreate s názvem skupiny NSG s názvem *NSG back-end* podle hello scénář výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="56fa2-152">toocreate an NSG named named *NSG-BackEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="56fa2-153">Spustit hello  **`azure network nsg create`**  příkaz toocreate skupinu NSG.</span><span class="sxs-lookup"><span data-stu-id="56fa2-153">Run hello **`azure network nsg create`** command toocreate an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    <span data-ttu-id="56fa2-154">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56fa2-154">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
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
   
    <span data-ttu-id="56fa2-155">Parametry:</span><span class="sxs-lookup"><span data-stu-id="56fa2-155">Parameters:</span></span>
   
   * <span data-ttu-id="56fa2-156">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-156">**-l (or --location)**.</span></span> <span data-ttu-id="56fa2-157">Oblast Azure, kde hello nová skupina NSG bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="56fa2-157">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="56fa2-158">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="56fa2-158">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="56fa2-159">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="56fa2-159">**-n (or --name)**.</span></span> <span data-ttu-id="56fa2-160">Název hello nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="56fa2-160">Name for hello new NSG.</span></span> <span data-ttu-id="56fa2-161">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="56fa2-161">For our scenario, *NSG-FrontEnd*.</span></span>
2. <span data-ttu-id="56fa2-162">Spustit hello  **`azure network nsg rule create`**  příkaz toocreate pravidlo, které umožní přístup tooport 1433 (SQL) z podsítě front end hello.</span><span class="sxs-lookup"><span data-stu-id="56fa2-162">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 1433 (SQL) from hello front end subnet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="56fa2-163">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56fa2-163">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
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
3. <span data-ttu-id="56fa2-164">Spustit hello  **`azure network nsg rule create`**  příkaz toocreate pravidlo, které zakazuje toohello přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="56fa2-164">Run hello **`azure network nsg rule create`** command toocreate a rule that denies access toohello Internet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    <span data-ttu-id="56fa2-165">Očekávaný putput:</span><span class="sxs-lookup"><span data-stu-id="56fa2-165">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
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
4. <span data-ttu-id="56fa2-166">Spustit hello  **`azure network nsg subnet add`**  příkaz toolink hello NSG toohello zpět ukončení podsítě.</span><span class="sxs-lookup"><span data-stu-id="56fa2-166">Run hello **`azure network nsg subnet add`** command toolink hello NSG toohello back end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    <span data-ttu-id="56fa2-167">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56fa2-167">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up hello network security group "NSG-BackEndX"
        info:    Looking up hello subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

