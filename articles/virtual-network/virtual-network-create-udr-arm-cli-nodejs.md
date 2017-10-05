---
title: "Řídit směrování a virtuální zařízení pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak řídit směrování a virtuální zařízení pomocí Azure CLI 1.0."
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
ms.openlocfilehash: 5f21bc7a4fcd9507ea9d6b2b752a2328a7b834f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-10"></a><span data-ttu-id="6ac2e-103">Vytvoření trasy definované uživatelem (UDR) pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="6ac2e-103">Create User-Defined Routes (UDR) using the Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6ac2e-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6ac2e-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="6ac2e-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6ac2e-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="6ac2e-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="6ac2e-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="6ac2e-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="6ac2e-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="6ac2e-108">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="6ac2e-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

<span data-ttu-id="6ac2e-109">Vytvořte vlastní směrování a virtuální zařízení pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-109">Create custom routing and virtual appliances using the Azure CLI.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="6ac2e-110">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="6ac2e-110">CLI versions to complete the task</span></span> 

<span data-ttu-id="6ac2e-111">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-111">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="6ac2e-112">[Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="6ac2e-112">[Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="6ac2e-113">[Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="6ac2e-113">[Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) - our next generation CLI for the resource management deployment model</span></span> 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="6ac2e-114">Níže uvedené příkazy rozhraní příkazového řádku Azure ukázka očekávat jednoduché prostředí již vytvořeny podle výše uvedené scénáře.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-114">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="6ac2e-115">Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nasazením nejprve vytvořit testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasadit do Azure**, nahradí výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů v portálu.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-115">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>


## <a name="create-the-udr-for-the-front-end-subnet"></a><span data-ttu-id="6ac2e-116">Vytvoření UDR front-end podsítě</span><span class="sxs-lookup"><span data-stu-id="6ac2e-116">Create the UDR for the front-end subnet</span></span>
<span data-ttu-id="6ac2e-117">Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřebné pro podsítě front end závislosti na scénáři výše, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-117">To create the route table and route needed for the front end subnet based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="6ac2e-118">Spusťte následující příkaz, který vytvořit směrovací tabulku front-end podsítě:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-118">Run the following command to create a route table for the front-end subnet:</span></span>

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    <span data-ttu-id="6ac2e-119">Výstup:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-119">Output:</span></span>
   
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
   
    <span data-ttu-id="6ac2e-120">Parametry:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-120">Parameters:</span></span>
   
   * <span data-ttu-id="6ac2e-121">**-g (nebo --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-121">**-g (or --resource-group)**.</span></span> <span data-ttu-id="6ac2e-122">Název skupiny prostředků, které UDR vytvoří.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-122">Name of the resource group where the UDR will be created.</span></span> <span data-ttu-id="6ac2e-123">V našem scénáři je to *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-123">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="6ac2e-124">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-124">**-l (or --location)**.</span></span> <span data-ttu-id="6ac2e-125">Oblast Azure, kde bude vytvořen nový UDR.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-125">Azure region where the new UDR will be created.</span></span> <span data-ttu-id="6ac2e-126">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-126">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="6ac2e-127">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-127">**-n (or --name)**.</span></span> <span data-ttu-id="6ac2e-128">Název pro nový UDR.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-128">Name for the new UDR.</span></span> <span data-ttu-id="6ac2e-129">Pro náš scénář *UDR front-endu*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-129">For our scenario, *UDR-FrontEnd*.</span></span>
2. <span data-ttu-id="6ac2e-130">Spusťte následující příkaz k vytvoření trasy ve směrovací tabulce odeslat veškerý provoz, jehož k podsíti back-end (192.168.2.0/24) na **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="6ac2e-130">Run the following command to create a route in the route table to send all traffic destined to the back-end subnet (192.168.2.0/24) to the **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    <span data-ttu-id="6ac2e-131">Výstup:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-131">Output:</span></span>
   
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
   
    <span data-ttu-id="6ac2e-132">Parametry:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-132">Parameters:</span></span>
   
   * <span data-ttu-id="6ac2e-133">**-r (nebo--název směrovací tabulky)**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-133">**-r (or --route-table-name)**.</span></span> <span data-ttu-id="6ac2e-134">Název směrovací tabulka, kam bude přidána trasy.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-134">Name of the route table where the route will be added.</span></span> <span data-ttu-id="6ac2e-135">Pro náš scénář *UDR front-endu*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-135">For our scenario, *UDR-FrontEnd*.</span></span>
   * <span data-ttu-id="6ac2e-136">**-a (nebo --address-prefixes)**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-136">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="6ac2e-137">Předpona adresy podsítě, kde jsou pakety určené do.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-137">Address prefix for the subnet where packets are destined to.</span></span> <span data-ttu-id="6ac2e-138">Pro náš scénář *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-138">For our scenario, *192.168.2.0/24*.</span></span>
   * <span data-ttu-id="6ac2e-139">**-y (nebo--další typ směrování)**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-139">**-y (or --next-hop-type)**.</span></span> <span data-ttu-id="6ac2e-140">Typ objektu provozu se odešle do.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-140">Type of object traffic will be sent to.</span></span> <span data-ttu-id="6ac2e-141">Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-141">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
   * <span data-ttu-id="6ac2e-142">**-p (nebo--další směrování ip adresu**).</span><span class="sxs-lookup"><span data-stu-id="6ac2e-142">**-p (or --next-hop-ip-address**).</span></span> <span data-ttu-id="6ac2e-143">IP adresa dalšího směrování.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-143">IP address for next hop.</span></span> <span data-ttu-id="6ac2e-144">Pro náš scénář *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-144">For our scenario, *192.168.0.4*.</span></span>
3. <span data-ttu-id="6ac2e-145">Spusťte následující příkaz k přiřazení směrovací tabulka vytvořili výše s **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-145">Run the following command to associate the route table created above with the **FrontEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    <span data-ttu-id="6ac2e-146">Výstup:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-146">Output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
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
   
    <span data-ttu-id="6ac2e-147">Parametry:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-147">Parameters:</span></span>
   
   * <span data-ttu-id="6ac2e-148">**-e (nebo--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-148">**-e (or --vnet-name)**.</span></span> <span data-ttu-id="6ac2e-149">Název sítě VNet, kde je umístěný v podsíti.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-149">Name of the VNet where the subnet is located.</span></span> <span data-ttu-id="6ac2e-150">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-150">For our scenario, *TestVNet*.</span></span>

## <a name="create-the-udr-for-the-back-end-subnet"></a><span data-ttu-id="6ac2e-151">Vytvoření UDR pro podsíť back-end</span><span class="sxs-lookup"><span data-stu-id="6ac2e-151">Create the UDR for the back-end subnet</span></span>
<span data-ttu-id="6ac2e-152">Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřeba pro back-end podsíť závislosti na scénáři výše, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-152">To create the route table and route needed for the back-end subnet based on the scenario above, complete the following steps:</span></span>

1. <span data-ttu-id="6ac2e-153">Spusťte následující příkaz a vytvořte tabulku směrování pro podsíť back-end:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-153">Run the following command to create a route table for the back-end subnet:</span></span>

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. <span data-ttu-id="6ac2e-154">Spusťte následující příkaz k vytvoření trasy ve směrovací tabulce odeslat veškerý provoz, jehož klientské podsíti (192.168.1.0/24) na **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="6ac2e-154">Run the following command to create a route in the route table to send all traffic destined to the front-end subnet (192.168.1.0/24) to the **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. <span data-ttu-id="6ac2e-155">Spusťte následující příkaz k přiřazení směrovací tabulka s **back-end** podsítě:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-155">Run the following command to associate the route table with the **BackEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="6ac2e-156">Povolení předávání IP na FW1</span><span class="sxs-lookup"><span data-stu-id="6ac2e-156">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="6ac2e-157">Povolení předávání IP v síťový adaptér používá **FW1**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-157">To enable IP forwarding in the NIC used by **FW1**, complete the following steps:</span></span>

1. <span data-ttu-id="6ac2e-158">Spustit příkaz, který následuje a Všimněte si, hodnota **předávání IP povolit**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-158">Run the command that follows and notice the value for **Enable IP forwarding**.</span></span> <span data-ttu-id="6ac2e-159">Musí být nastavena na *false*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-159">It should be set to *false*.</span></span>

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    <span data-ttu-id="6ac2e-160">Výstup:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-160">Output:</span></span>
   
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
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
2. <span data-ttu-id="6ac2e-161">Spusťte následující příkaz pro povolení předávání IP:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-161">Run the following command to enable IP forwarding:</span></span>

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    <span data-ttu-id="6ac2e-162">Výstup:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-162">Output:</span></span>
   
        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
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
   
    <span data-ttu-id="6ac2e-163">Parametry:</span><span class="sxs-lookup"><span data-stu-id="6ac2e-163">Parameters:</span></span>
   
   * <span data-ttu-id="6ac2e-164">**-f (nebo--povolení předávání ip)**.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-164">**-f (or --enable-ip-forwarding)**.</span></span> <span data-ttu-id="6ac2e-165">*Hodnota TRUE,* nebo *false*.</span><span class="sxs-lookup"><span data-stu-id="6ac2e-165">*true* or *false*.</span></span>

