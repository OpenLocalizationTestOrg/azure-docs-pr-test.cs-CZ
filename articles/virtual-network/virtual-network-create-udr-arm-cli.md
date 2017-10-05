---
title: "Řídit směrování a virtuální zařízení pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak řídit směrování a virtuální zařízení pomocí Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
ms.openlocfilehash: e5d9519998346619093f443b740c8904283f76e8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-20"></a><span data-ttu-id="ddf54-103">Vytvoření trasy definované uživatelem (UDR) pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ddf54-103">Create User-Defined Routes (UDR) using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ddf54-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ddf54-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="ddf54-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ddf54-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="ddf54-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="ddf54-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="ddf54-107">Prostředí PowerShell (nasazení Classic)</span><span class="sxs-lookup"><span data-stu-id="ddf54-107">PowerShell (Classic deployment)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="ddf54-108">Rozhraní příkazového řádku (nasazení Classic)</span><span class="sxs-lookup"><span data-stu-id="ddf54-108">CLI (Classic deployment)</span></span>](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="ddf54-109">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="ddf54-109">CLI versions to complete the task</span></span> 

<span data-ttu-id="ddf54-110">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="ddf54-110">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="ddf54-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="ddf54-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – our CLI for the classic and resource management deployment models</span></span> 
- <span data-ttu-id="ddf54-112">[Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -naší nové generace rozhraní příkazového řádku pro správu model nasazení prostředku (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="ddf54-112">[Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) - our next generation CLI for the resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="ddf54-113">Níže uvedené příkazy rozhraní příkazového řádku Azure ukázka očekávat jednoduché prostředí již vytvořeny podle výše uvedené scénáře.</span><span class="sxs-lookup"><span data-stu-id="ddf54-113">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="ddf54-114">Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nasazením nejprve vytvořit testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasadit do Azure**, nahradí výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů v portálu.</span><span class="sxs-lookup"><span data-stu-id="ddf54-114">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>


## <a name="create-the-udr-for-the-front-end-subnet"></a><span data-ttu-id="ddf54-115">Vytvoření UDR front-end podsítě</span><span class="sxs-lookup"><span data-stu-id="ddf54-115">Create the UDR for the front-end subnet</span></span>
<span data-ttu-id="ddf54-116">Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřebné pro podsítě front end závislosti na scénáři výše, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="ddf54-116">To create the route table and route needed for the front end subnet based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="ddf54-117">Vytvořit směrovací tabulku pro front-endu podsíť s [vytvoření tabulky trasy sítě az](/cli/azure/network/route-table#create) příkaz:</span><span class="sxs-lookup"><span data-stu-id="ddf54-117">Create a route table for the front-end subnet with the [az network route-table create](/cli/azure/network/route-table#create) command:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    <span data-ttu-id="ddf54-118">Výstup:</span><span class="sxs-lookup"><span data-stu-id="ddf54-118">Output:</span></span>

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. <span data-ttu-id="ddf54-119">Vytvořit trasu, která odesílá veškerý provoz, jehož k podsíti back-end (192.168.2.0/24) **FW1** virtuální počítač (192.168.0.4) pomocí [az síťovou směrovací tabulku trasu vytvořit](/cli/azure/network/route-table/route#create) příkaz:</span><span class="sxs-lookup"><span data-stu-id="ddf54-119">Create a route that sends all traffic destined to the back-end subnet (192.168.2.0/24) to the **FW1** VM (192.168.0.4) using the [az network route-table route create](/cli/azure/network/route-table/route#create) command:</span></span>

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    <span data-ttu-id="ddf54-120">Výstup:</span><span class="sxs-lookup"><span data-stu-id="ddf54-120">Output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    <span data-ttu-id="ddf54-121">Parametry:</span><span class="sxs-lookup"><span data-stu-id="ddf54-121">Parameters:</span></span>

    * <span data-ttu-id="ddf54-122">**– Název směrovací tabulky**.</span><span class="sxs-lookup"><span data-stu-id="ddf54-122">**--route-table-name**.</span></span> <span data-ttu-id="ddf54-123">Název směrovací tabulka, kam bude přidána trasy.</span><span class="sxs-lookup"><span data-stu-id="ddf54-123">Name of the route table where the route will be added.</span></span> <span data-ttu-id="ddf54-124">Pro náš scénář *UDR front-endu*.</span><span class="sxs-lookup"><span data-stu-id="ddf54-124">For our scenario, *UDR-FrontEnd*.</span></span>
    * <span data-ttu-id="ddf54-125">**--address prefixes**.</span><span class="sxs-lookup"><span data-stu-id="ddf54-125">**--address-prefix**.</span></span> <span data-ttu-id="ddf54-126">Předpona adresy podsítě, kde jsou pakety určené do.</span><span class="sxs-lookup"><span data-stu-id="ddf54-126">Address prefix for the subnet where packets are destined to.</span></span> <span data-ttu-id="ddf54-127">Pro náš scénář *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="ddf54-127">For our scenario, *192.168.2.0/24*.</span></span>
    * <span data-ttu-id="ddf54-128">**--Další typ směrování**.</span><span class="sxs-lookup"><span data-stu-id="ddf54-128">**--next-hop-type**.</span></span> <span data-ttu-id="ddf54-129">Typ objektu provozu se odešle do.</span><span class="sxs-lookup"><span data-stu-id="ddf54-129">Type of object traffic will be sent to.</span></span> <span data-ttu-id="ddf54-130">Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.</span><span class="sxs-lookup"><span data-stu-id="ddf54-130">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
    * <span data-ttu-id="ddf54-131">**--Další směrování ip adresu**.</span><span class="sxs-lookup"><span data-stu-id="ddf54-131">**--next-hop-ip-address**.</span></span> <span data-ttu-id="ddf54-132">IP adresa dalšího směrování.</span><span class="sxs-lookup"><span data-stu-id="ddf54-132">IP address for next hop.</span></span> <span data-ttu-id="ddf54-133">Pro náš scénář *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="ddf54-133">For our scenario, *192.168.0.4*.</span></span>

3. <span data-ttu-id="ddf54-134">Spustit [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) příkaz přidružit směrovací tabulka vytvořili výše s **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="ddf54-134">Run the [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command to associate the route table created above with the **FrontEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    <span data-ttu-id="ddf54-135">Výstup:</span><span class="sxs-lookup"><span data-stu-id="ddf54-135">Output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    <span data-ttu-id="ddf54-136">Parametry:</span><span class="sxs-lookup"><span data-stu-id="ddf54-136">Parameters:</span></span>
    
    * <span data-ttu-id="ddf54-137">**--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="ddf54-137">**--vnet-name**.</span></span> <span data-ttu-id="ddf54-138">Název sítě VNet, kde je umístěný v podsíti.</span><span class="sxs-lookup"><span data-stu-id="ddf54-138">Name of the VNet where the subnet is located.</span></span> <span data-ttu-id="ddf54-139">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="ddf54-139">For our scenario, *TestVNet*.</span></span>

## <a name="create-the-udr-for-the-back-end-subnet"></a><span data-ttu-id="ddf54-140">Vytvoření UDR pro podsíť back-end</span><span class="sxs-lookup"><span data-stu-id="ddf54-140">Create the UDR for the back-end subnet</span></span>

<span data-ttu-id="ddf54-141">Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřeba pro back-end podsíť závislosti na scénáři výše, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ddf54-141">To create the route table and route needed for the back-end subnet based on the scenario above, complete the following steps:</span></span>

1. <span data-ttu-id="ddf54-142">Spusťte následující příkaz a vytvořte tabulku směrování pro podsíť back-end:</span><span class="sxs-lookup"><span data-stu-id="ddf54-142">Run the following command to create a route table for the back-end subnet:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. <span data-ttu-id="ddf54-143">Spusťte následující příkaz k vytvoření trasy ve směrovací tabulce odeslat veškerý provoz, jehož klientské podsíti (192.168.1.0/24) na **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="ddf54-143">Run the following command to create a route in the route table to send all traffic destined to the front-end subnet (192.168.1.0/24) to the **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. <span data-ttu-id="ddf54-144">Spusťte následující příkaz k přiřazení směrovací tabulka s **back-end** podsítě:</span><span class="sxs-lookup"><span data-stu-id="ddf54-144">Run the following command to associate the route table with the **BackEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="ddf54-145">Povolení předávání IP na FW1</span><span class="sxs-lookup"><span data-stu-id="ddf54-145">Enable IP forwarding on FW1</span></span>

<span data-ttu-id="ddf54-146">Povolení předávání IP v síťový adaptér používá **FW1**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ddf54-146">To enable IP forwarding in the NIC used by **FW1**, complete the following steps:</span></span>

1. <span data-ttu-id="ddf54-147">Spustit [az sítě seskupování zobrazit](/cli/azure/network/nic#show) příkazu s filtrem JMESPATH zobrazíte aktuální **povolení předávání ip** hodnota **předávání IP povolit**.</span><span class="sxs-lookup"><span data-stu-id="ddf54-147">Run the [az network nic show](/cli/azure/network/nic#show) command with a JMESPATH filter to display the current **enable-ip-forwarding** value for **Enable IP forwarding**.</span></span> <span data-ttu-id="ddf54-148">Musí být nastavena na *false*.</span><span class="sxs-lookup"><span data-stu-id="ddf54-148">It should be set to *false*.</span></span>

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="ddf54-149">Výstup:</span><span class="sxs-lookup"><span data-stu-id="ddf54-149">Output:</span></span>

        false

2. <span data-ttu-id="ddf54-150">Spusťte následující příkaz pro povolení předávání IP:</span><span class="sxs-lookup"><span data-stu-id="ddf54-150">Run the following command to enable IP forwarding:</span></span>

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    <span data-ttu-id="ddf54-151">Vyhledejte ve výstupu datového proudu ke konzole nebo jenom pro konkrétní testování **enableIpForwarding** hodnotu:</span><span class="sxs-lookup"><span data-stu-id="ddf54-151">You can examine the output streamed to the console, or just retest for the specific **enableIpForwarding** value:</span></span>

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="ddf54-152">Výstup:</span><span class="sxs-lookup"><span data-stu-id="ddf54-152">Output:</span></span>

        true

    <span data-ttu-id="ddf54-153">Parametry:</span><span class="sxs-lookup"><span data-stu-id="ddf54-153">Parameters:</span></span>

    <span data-ttu-id="ddf54-154">**předávání--ip**: *true* nebo *false*.</span><span class="sxs-lookup"><span data-stu-id="ddf54-154">**--ip-forwarding**: *true* or *false*.</span></span>

