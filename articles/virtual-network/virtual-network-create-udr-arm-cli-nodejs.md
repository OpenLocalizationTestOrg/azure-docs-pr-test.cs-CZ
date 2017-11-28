---
title: "Směrování a virtuální zařízení aaaControl pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocontrol směrování a virtuální zařízení pomocí Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 1c8a552d949521fa554880c00405e65fa47a8162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-10"></a><span data-ttu-id="19636-103">Vytvoření trasy definované uživatelem (UDR) pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="19636-103">Create User-Defined Routes (UDR) using hello Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="19636-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="19636-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="19636-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="19636-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="19636-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="19636-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="19636-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="19636-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="19636-108">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="19636-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

<span data-ttu-id="19636-109">Vytvořte vlastní směrování a virtuální zařízení pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="19636-109">Create custom routing and virtual appliances using hello Azure CLI.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="19636-110">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="19636-110">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="19636-111">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="19636-111">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="19636-112">[Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="19636-112">[Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="19636-113">[Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="19636-113">[Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) - our next generation CLI for hello resource management deployment model</span></span> 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="19636-114">níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello výše.</span><span class="sxs-lookup"><span data-stu-id="19636-114">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="19636-115">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření nasazením nejprve hello testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů Pokud potřeby a postupujte podle pokynů hello v hello portálu.</span><span class="sxs-lookup"><span data-stu-id="19636-115">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>


## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="19636-116">Vytvoření hello UDR podsítě front-endu hello</span><span class="sxs-lookup"><span data-stu-id="19636-116">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="19636-117">toocreate hello směrovací tabulku a směrování, které jsou potřebné pro podsítě front end hello podle hello scénář výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="19636-117">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="19636-118">Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť pro front-end hello:</span><span class="sxs-lookup"><span data-stu-id="19636-118">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    <span data-ttu-id="19636-119">Výstup:</span><span class="sxs-lookup"><span data-stu-id="19636-119">Output:</span></span>
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    <span data-ttu-id="19636-120">Parametry:</span><span class="sxs-lookup"><span data-stu-id="19636-120">Parameters:</span></span>
   
   * <span data-ttu-id="19636-121">**-g (nebo --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="19636-121">**-g (or --resource-group)**.</span></span> <span data-ttu-id="19636-122">Název skupiny prostředků hello, kde bude vytvořen hello UDR.</span><span class="sxs-lookup"><span data-stu-id="19636-122">Name of hello resource group where hello UDR will be created.</span></span> <span data-ttu-id="19636-123">V našem scénáři je to *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="19636-123">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="19636-124">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="19636-124">**-l (or --location)**.</span></span> <span data-ttu-id="19636-125">Oblast Azure, kde hello nové UDR bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="19636-125">Azure region where hello new UDR will be created.</span></span> <span data-ttu-id="19636-126">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="19636-126">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="19636-127">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="19636-127">**-n (or --name)**.</span></span> <span data-ttu-id="19636-128">Název hello nové UDR.</span><span class="sxs-lookup"><span data-stu-id="19636-128">Name for hello new UDR.</span></span> <span data-ttu-id="19636-129">Pro náš scénář *UDR front-endu*.</span><span class="sxs-lookup"><span data-stu-id="19636-129">For our scenario, *UDR-FrontEnd*.</span></span>
2. <span data-ttu-id="19636-130">Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello podsítě back-end (192.168.2.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="19636-130">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    <span data-ttu-id="19636-131">Výstup:</span><span class="sxs-lookup"><span data-stu-id="19636-131">Output:</span></span>
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    <span data-ttu-id="19636-132">Parametry:</span><span class="sxs-lookup"><span data-stu-id="19636-132">Parameters:</span></span>
   
   * <span data-ttu-id="19636-133">**-r (nebo--název směrovací tabulky)**.</span><span class="sxs-lookup"><span data-stu-id="19636-133">**-r (or --route-table-name)**.</span></span> <span data-ttu-id="19636-134">Název hello směrovací tabulka, kam bude přidána hello trasy.</span><span class="sxs-lookup"><span data-stu-id="19636-134">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="19636-135">Pro náš scénář *UDR front-endu*.</span><span class="sxs-lookup"><span data-stu-id="19636-135">For our scenario, *UDR-FrontEnd*.</span></span>
   * <span data-ttu-id="19636-136">**-a (nebo --address-prefixes)**.</span><span class="sxs-lookup"><span data-stu-id="19636-136">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="19636-137">Předpona adresy podsítě hello, kde jsou pakety určené do.</span><span class="sxs-lookup"><span data-stu-id="19636-137">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="19636-138">Pro náš scénář *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="19636-138">For our scenario, *192.168.2.0/24*.</span></span>
   * <span data-ttu-id="19636-139">**-y (nebo--další typ směrování)**.</span><span class="sxs-lookup"><span data-stu-id="19636-139">**-y (or --next-hop-type)**.</span></span> <span data-ttu-id="19636-140">Typ objektu provozu se odešle do.</span><span class="sxs-lookup"><span data-stu-id="19636-140">Type of object traffic will be sent to.</span></span> <span data-ttu-id="19636-141">Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.</span><span class="sxs-lookup"><span data-stu-id="19636-141">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
   * <span data-ttu-id="19636-142">**-p (nebo--další směrování ip adresu**).</span><span class="sxs-lookup"><span data-stu-id="19636-142">**-p (or --next-hop-ip-address**).</span></span> <span data-ttu-id="19636-143">IP adresa dalšího směrování.</span><span class="sxs-lookup"><span data-stu-id="19636-143">IP address for next hop.</span></span> <span data-ttu-id="19636-144">Pro náš scénář *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="19636-144">For our scenario, *192.168.0.4*.</span></span>
3. <span data-ttu-id="19636-145">Hello spusťte následující příkaz tooassociate hello směrovací tabulku vytvořili výše s hello **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="19636-145">Run hello following command tooassociate hello route table created above with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    <span data-ttu-id="19636-146">Výstup:</span><span class="sxs-lookup"><span data-stu-id="19636-146">Output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    <span data-ttu-id="19636-147">Parametry:</span><span class="sxs-lookup"><span data-stu-id="19636-147">Parameters:</span></span>
   
   * <span data-ttu-id="19636-148">**-e (nebo--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="19636-148">**-e (or --vnet-name)**.</span></span> <span data-ttu-id="19636-149">Název hello virtuální síť, kde se nachází hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="19636-149">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="19636-150">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="19636-150">For our scenario, *TestVNet*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="19636-151">Vytvoření hello UDR pro podsíť back-end hello</span><span class="sxs-lookup"><span data-stu-id="19636-151">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="19636-152">toocreate hello směrovací tabulky a trasy potřebné pro podsíť back-end hello podle hello scénář výše, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="19636-152">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="19636-153">Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť back-end hello:</span><span class="sxs-lookup"><span data-stu-id="19636-153">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. <span data-ttu-id="19636-154">Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello front-end podsíť (192.168.1.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="19636-154">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. <span data-ttu-id="19636-155">Spuštění hello následující příkaz tooassociate hello směrovací tabulku s hello **back-end** podsítě:</span><span class="sxs-lookup"><span data-stu-id="19636-155">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="19636-156">Povolení předávání IP na FW1</span><span class="sxs-lookup"><span data-stu-id="19636-156">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="19636-157">předávání IP tooenable v hello síťový adaptér používá **FW1**, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="19636-157">tooenable IP forwarding in hello NIC used by **FW1**, complete hello following steps:</span></span>

1. <span data-ttu-id="19636-158">Spusťte příkaz hello, který následuje a Všimněte si hello hodnotu pro **předávání IP povolit**.</span><span class="sxs-lookup"><span data-stu-id="19636-158">Run hello command that follows and notice hello value for **Enable IP forwarding**.</span></span> <span data-ttu-id="19636-159">Je potřeba ho nastavit příliš*false*.</span><span class="sxs-lookup"><span data-stu-id="19636-159">It should be set too*false*.</span></span>

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    <span data-ttu-id="19636-160">Výstup:</span><span class="sxs-lookup"><span data-stu-id="19636-160">Output:</span></span>
   
        info:    Executing command network nic show
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. <span data-ttu-id="19636-161">Spusťte následující příkaz předávání IP tooenable hello:</span><span class="sxs-lookup"><span data-stu-id="19636-161">Run hello following command tooenable IP forwarding:</span></span>

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    <span data-ttu-id="19636-162">Výstup:</span><span class="sxs-lookup"><span data-stu-id="19636-162">Output:</span></span>
   
        info:    Executing command network nic set
        info:    Looking up hello network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    <span data-ttu-id="19636-163">Parametry:</span><span class="sxs-lookup"><span data-stu-id="19636-163">Parameters:</span></span>
   
   * <span data-ttu-id="19636-164">**-f (nebo--povolení předávání ip)**.</span><span class="sxs-lookup"><span data-stu-id="19636-164">**-f (or --enable-ip-forwarding)**.</span></span> <span data-ttu-id="19636-165">*Hodnota TRUE,* nebo *false*.</span><span class="sxs-lookup"><span data-stu-id="19636-165">*true* or *false*.</span></span>

