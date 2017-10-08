---
title: "Směrování a virtuální zařízení aaaControl pomocí hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocontrol směrování a virtuální zařízení pomocí Azure CLI 2.0."
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
ms.openlocfilehash: 79b908848932a4365dab1b7497b6a0dbf44bbaf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-20"></a><span data-ttu-id="333fa-103">Vytvoření trasy definované uživatelem (UDR) pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="333fa-103">Create User-Defined Routes (UDR) using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="333fa-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="333fa-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="333fa-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="333fa-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="333fa-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="333fa-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="333fa-107">Prostředí PowerShell (nasazení Classic)</span><span class="sxs-lookup"><span data-stu-id="333fa-107">PowerShell (Classic deployment)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="333fa-108">Rozhraní příkazového řádku (nasazení Classic)</span><span class="sxs-lookup"><span data-stu-id="333fa-108">CLI (Classic deployment)</span></span>](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="333fa-109">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="333fa-109">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="333fa-110">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="333fa-110">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="333fa-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely</span><span class="sxs-lookup"><span data-stu-id="333fa-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="333fa-112">[Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="333fa-112">[Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="333fa-113">níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello výše.</span><span class="sxs-lookup"><span data-stu-id="333fa-113">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="333fa-114">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření nasazením nejprve hello testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů Pokud potřeby a postupujte podle pokynů hello v hello portálu.</span><span class="sxs-lookup"><span data-stu-id="333fa-114">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>


## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="333fa-115">Vytvoření hello UDR podsítě front-endu hello</span><span class="sxs-lookup"><span data-stu-id="333fa-115">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="333fa-116">toocreate hello směrovací tabulku a směrování, které jsou potřebné pro podsítě front end hello podle hello scénář výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="333fa-116">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="333fa-117">Vytvořit směrovací tabulku pro hello front-end podsíť s hello [vytvoření tabulky trasy sítě az](/cli/azure/network/route-table#create) příkaz:</span><span class="sxs-lookup"><span data-stu-id="333fa-117">Create a route table for hello front-end subnet with hello [az network route-table create](/cli/azure/network/route-table#create) command:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    <span data-ttu-id="333fa-118">Výstup:</span><span class="sxs-lookup"><span data-stu-id="333fa-118">Output:</span></span>

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

2. <span data-ttu-id="333fa-119">Vytvoření cesty, která odesílá všechny přenosy určené toohello podsítě back-end (192.168.2.0/24) toohello **FW1** virtuálního počítače (192.168.0.4) pomocí hello [az síťovou směrovací tabulku trasu vytvořit](/cli/azure/network/route-table/route#create) příkaz:</span><span class="sxs-lookup"><span data-stu-id="333fa-119">Create a route that sends all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4) using hello [az network route-table route create](/cli/azure/network/route-table/route#create) command:</span></span>

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    <span data-ttu-id="333fa-120">Výstup:</span><span class="sxs-lookup"><span data-stu-id="333fa-120">Output:</span></span>

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
    <span data-ttu-id="333fa-121">Parametry:</span><span class="sxs-lookup"><span data-stu-id="333fa-121">Parameters:</span></span>

    * <span data-ttu-id="333fa-122">**– Název směrovací tabulky**.</span><span class="sxs-lookup"><span data-stu-id="333fa-122">**--route-table-name**.</span></span> <span data-ttu-id="333fa-123">Název hello směrovací tabulka, kam bude přidána hello trasy.</span><span class="sxs-lookup"><span data-stu-id="333fa-123">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="333fa-124">Pro náš scénář *UDR front-endu*.</span><span class="sxs-lookup"><span data-stu-id="333fa-124">For our scenario, *UDR-FrontEnd*.</span></span>
    * <span data-ttu-id="333fa-125">**--address prefixes**.</span><span class="sxs-lookup"><span data-stu-id="333fa-125">**--address-prefix**.</span></span> <span data-ttu-id="333fa-126">Předpona adresy podsítě hello, kde jsou pakety určené do.</span><span class="sxs-lookup"><span data-stu-id="333fa-126">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="333fa-127">Pro náš scénář *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="333fa-127">For our scenario, *192.168.2.0/24*.</span></span>
    * <span data-ttu-id="333fa-128">**--Další typ směrování**.</span><span class="sxs-lookup"><span data-stu-id="333fa-128">**--next-hop-type**.</span></span> <span data-ttu-id="333fa-129">Typ objektu provozu se odešle do.</span><span class="sxs-lookup"><span data-stu-id="333fa-129">Type of object traffic will be sent to.</span></span> <span data-ttu-id="333fa-130">Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.</span><span class="sxs-lookup"><span data-stu-id="333fa-130">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
    * <span data-ttu-id="333fa-131">**--Další směrování ip adresu**.</span><span class="sxs-lookup"><span data-stu-id="333fa-131">**--next-hop-ip-address**.</span></span> <span data-ttu-id="333fa-132">IP adresa dalšího směrování.</span><span class="sxs-lookup"><span data-stu-id="333fa-132">IP address for next hop.</span></span> <span data-ttu-id="333fa-133">Pro náš scénář *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="333fa-133">For our scenario, *192.168.0.4*.</span></span>

3. <span data-ttu-id="333fa-134">Spustit hello [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) příkaz tooassociate hello směrovací tabulku vytvořili výše s hello **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="333fa-134">Run hello [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command tooassociate hello route table created above with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    <span data-ttu-id="333fa-135">Výstup:</span><span class="sxs-lookup"><span data-stu-id="333fa-135">Output:</span></span>

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

    <span data-ttu-id="333fa-136">Parametry:</span><span class="sxs-lookup"><span data-stu-id="333fa-136">Parameters:</span></span>
    
    * <span data-ttu-id="333fa-137">**--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="333fa-137">**--vnet-name**.</span></span> <span data-ttu-id="333fa-138">Název hello virtuální síť, kde se nachází hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="333fa-138">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="333fa-139">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="333fa-139">For our scenario, *TestVNet*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="333fa-140">Vytvoření hello UDR pro podsíť back-end hello</span><span class="sxs-lookup"><span data-stu-id="333fa-140">Create hello UDR for hello back-end subnet</span></span>

<span data-ttu-id="333fa-141">toocreate hello směrovací tabulky a trasy potřebné pro podsíť back-end hello podle hello scénář výše, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="333fa-141">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="333fa-142">Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť back-end hello:</span><span class="sxs-lookup"><span data-stu-id="333fa-142">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. <span data-ttu-id="333fa-143">Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello front-end podsíť (192.168.1.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="333fa-143">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. <span data-ttu-id="333fa-144">Spuštění hello následující příkaz tooassociate hello směrovací tabulku s hello **back-end** podsítě:</span><span class="sxs-lookup"><span data-stu-id="333fa-144">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="333fa-145">Povolení předávání IP na FW1</span><span class="sxs-lookup"><span data-stu-id="333fa-145">Enable IP forwarding on FW1</span></span>

<span data-ttu-id="333fa-146">předávání IP tooenable v hello síťový adaptér používá **FW1**, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="333fa-146">tooenable IP forwarding in hello NIC used by **FW1**, complete hello following steps:</span></span>

1. <span data-ttu-id="333fa-147">Spustit hello [az sítě seskupování zobrazit](/cli/azure/network/nic#show) s aktuální JMESPATH hello toodisplay filtru **povolení předávání ip** hodnota **předávání IP povolit**.</span><span class="sxs-lookup"><span data-stu-id="333fa-147">Run hello [az network nic show](/cli/azure/network/nic#show) command with a JMESPATH filter toodisplay hello current **enable-ip-forwarding** value for **Enable IP forwarding**.</span></span> <span data-ttu-id="333fa-148">Je potřeba ho nastavit příliš*false*.</span><span class="sxs-lookup"><span data-stu-id="333fa-148">It should be set too*false*.</span></span>

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="333fa-149">Výstup:</span><span class="sxs-lookup"><span data-stu-id="333fa-149">Output:</span></span>

        false

2. <span data-ttu-id="333fa-150">Spusťte následující příkaz předávání IP tooenable hello:</span><span class="sxs-lookup"><span data-stu-id="333fa-150">Run hello following command tooenable IP forwarding:</span></span>

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    <span data-ttu-id="333fa-151">Můžete prozkoumat konzoly přenášené datovými proudy toohello výstup hello nebo jenom pro konkrétní hello testování **enableIpForwarding** hodnotu:</span><span class="sxs-lookup"><span data-stu-id="333fa-151">You can examine hello output streamed toohello console, or just retest for hello specific **enableIpForwarding** value:</span></span>

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="333fa-152">Výstup:</span><span class="sxs-lookup"><span data-stu-id="333fa-152">Output:</span></span>

        true

    <span data-ttu-id="333fa-153">Parametry:</span><span class="sxs-lookup"><span data-stu-id="333fa-153">Parameters:</span></span>

    <span data-ttu-id="333fa-154">**předávání--ip**: *true* nebo *false*.</span><span class="sxs-lookup"><span data-stu-id="333fa-154">**--ip-forwarding**: *true* or *false*.</span></span>

