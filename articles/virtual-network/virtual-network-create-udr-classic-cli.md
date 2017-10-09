---
title: "aaaControl směrování v virtuální síti příkazového - řádku - Azure Classic | Microsoft Docs"
description: "Zjistěte, jak hello toocontrol směrování v virtuální sítě pomocí rozhraní příkazového řádku Azure v modelu nasazení classic hello"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 07dde573f1a605bf280156c261d51e213ede0cdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-hello-azure-cli"></a><span data-ttu-id="9639b-103">Ovládací prvek směrování a použití virtuálních zařízení (klasické) pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="9639b-103">Control routing and use virtual appliances (classic) using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9639b-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9639b-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="9639b-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9639b-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="9639b-106">Šablona</span><span class="sxs-lookup"><span data-stu-id="9639b-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="9639b-107">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="9639b-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="9639b-108">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="9639b-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="9639b-109">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="9639b-109">This article covers hello classic deployment model.</span></span> <span data-ttu-id="9639b-110">Můžete také [řídit směrování a použití virtuálních zařízení v modelu nasazení Resource Manager hello](virtual-network-create-udr-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="9639b-110">You can also [control routing and use virtual appliances in hello Resource Manager deployment model](virtual-network-create-udr-arm-cli.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="9639b-111">níže uvedené příkazy rozhraní příkazového řádku Azure ukázkové Hello očekávat jednoduché prostředí již vytvořili závislosti na scénáři hello výše.</span><span class="sxs-lookup"><span data-stu-id="9639b-111">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="9639b-112">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření prostředí hello ukazuje [vytvoření virtuální sítě (klasické) pomocí rozhraní příkazového řádku Azure hello](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="9639b-112">If you want toorun hello commands as they are displayed in this document, create hello environment shown in [create a VNet (classic) using hello Azure CLI](virtual-networks-create-vnet-classic-cli.md).</span></span>

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="9639b-113">Vytvoření hello UDR pro podsítě front end hello</span><span class="sxs-lookup"><span data-stu-id="9639b-113">Create hello UDR for hello front end subnet</span></span>
<span data-ttu-id="9639b-114">toocreate hello směrovací tabulku a směrování, které jsou potřebné pro podsítě front end hello podle hello scénář výše, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="9639b-114">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="9639b-115">Spusťte následující příkaz tooswitch tooclassic režimu hello:</span><span class="sxs-lookup"><span data-stu-id="9639b-115">Run hello following command tooswitch tooclassic mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="9639b-116">Výstup:</span><span class="sxs-lookup"><span data-stu-id="9639b-116">Output:</span></span>

        info:    New mode is asm

2. <span data-ttu-id="9639b-117">Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť pro front-end hello:</span><span class="sxs-lookup"><span data-stu-id="9639b-117">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    <span data-ttu-id="9639b-118">Výstup:</span><span class="sxs-lookup"><span data-stu-id="9639b-118">Output:</span></span>
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    <span data-ttu-id="9639b-119">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9639b-119">Parameters:</span></span>
   
   * <span data-ttu-id="9639b-120">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="9639b-120">**-l (or --location)**.</span></span> <span data-ttu-id="9639b-121">Oblast Azure, kde hello nová skupina NSG bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="9639b-121">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="9639b-122">Pro náš scénář *westus*.</span><span class="sxs-lookup"><span data-stu-id="9639b-122">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="9639b-123">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="9639b-123">**-n (or --name)**.</span></span> <span data-ttu-id="9639b-124">Název hello nová skupina NSG.</span><span class="sxs-lookup"><span data-stu-id="9639b-124">Name for hello new NSG.</span></span> <span data-ttu-id="9639b-125">Pro náš scénář *NSG front-endu*.</span><span class="sxs-lookup"><span data-stu-id="9639b-125">For our scenario, *NSG-FrontEnd*.</span></span>
3. <span data-ttu-id="9639b-126">Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello podsítě back-end (192.168.2.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="9639b-126">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    <span data-ttu-id="9639b-127">Výstup:</span><span class="sxs-lookup"><span data-stu-id="9639b-127">Output:</span></span>
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    <span data-ttu-id="9639b-128">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9639b-128">Parameters:</span></span>
   
   * <span data-ttu-id="9639b-129">**-r (nebo--název směrovací tabulky)**.</span><span class="sxs-lookup"><span data-stu-id="9639b-129">**-r (or --route-table-name)**.</span></span> <span data-ttu-id="9639b-130">Název hello směrovací tabulka, kam bude přidána hello trasy.</span><span class="sxs-lookup"><span data-stu-id="9639b-130">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="9639b-131">Pro náš scénář *UDR front-endu*.</span><span class="sxs-lookup"><span data-stu-id="9639b-131">For our scenario, *UDR-FrontEnd*.</span></span>
   * <span data-ttu-id="9639b-132">**-a (nebo --address-prefixes)**.</span><span class="sxs-lookup"><span data-stu-id="9639b-132">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="9639b-133">Předpona adresy podsítě hello, kde jsou pakety určené do.</span><span class="sxs-lookup"><span data-stu-id="9639b-133">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="9639b-134">Pro náš scénář *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="9639b-134">For our scenario, *192.168.2.0/24*.</span></span>
   * <span data-ttu-id="9639b-135">**-t (nebo--další typ směrování)**.</span><span class="sxs-lookup"><span data-stu-id="9639b-135">**-t (or --next-hop-type)**.</span></span> <span data-ttu-id="9639b-136">Typ objektu provozu se odešle do.</span><span class="sxs-lookup"><span data-stu-id="9639b-136">Type of object traffic will be sent to.</span></span> <span data-ttu-id="9639b-137">Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.</span><span class="sxs-lookup"><span data-stu-id="9639b-137">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
   * <span data-ttu-id="9639b-138">**-p (nebo--další směrování ip adresu**).</span><span class="sxs-lookup"><span data-stu-id="9639b-138">**-p (or --next-hop-ip-address**).</span></span> <span data-ttu-id="9639b-139">IP adresa dalšího směrování.</span><span class="sxs-lookup"><span data-stu-id="9639b-139">IP address for next hop.</span></span> <span data-ttu-id="9639b-140">Pro náš scénář *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="9639b-140">For our scenario, *192.168.0.4*.</span></span>
4. <span data-ttu-id="9639b-141">Hello spusťte následující příkaz tooassociate hello směrovací tabulku vytvořené pomocí hello **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="9639b-141">Run hello following command tooassociate hello route table created with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    <span data-ttu-id="9639b-142">Výstup:</span><span class="sxs-lookup"><span data-stu-id="9639b-142">Output:</span></span>
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    <span data-ttu-id="9639b-143">Parametry:</span><span class="sxs-lookup"><span data-stu-id="9639b-143">Parameters:</span></span>
   
   * <span data-ttu-id="9639b-144">**-t (nebo--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="9639b-144">**-t (or --vnet-name)**.</span></span> <span data-ttu-id="9639b-145">Název hello virtuální síť, kde se nachází hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="9639b-145">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="9639b-146">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="9639b-146">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="9639b-147">**-n (nebo--název podsítě**.</span><span class="sxs-lookup"><span data-stu-id="9639b-147">**-n (or --subnet-name**.</span></span> <span data-ttu-id="9639b-148">Název hello podsíť hello směrovací tabulka se zařadí do.</span><span class="sxs-lookup"><span data-stu-id="9639b-148">Name of hello subnet hello route table will be added to.</span></span> <span data-ttu-id="9639b-149">V našem scénáři je to *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="9639b-149">For our scenario, *FrontEnd*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="9639b-150">Vytvoření hello UDR pro podsíť back-end hello</span><span class="sxs-lookup"><span data-stu-id="9639b-150">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="9639b-151">toocreate hello směrovací tabulku a směrování, které jsou potřebné pro podsíť back-end hello podle hello scénáři dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9639b-151">toocreate hello route table and route needed for hello back-end subnet based on hello scenario, complete hello following steps:</span></span>

1. <span data-ttu-id="9639b-152">Spusťte následující příkaz toocreate hello tabulka směrování pro podsíť back-end hello:</span><span class="sxs-lookup"><span data-stu-id="9639b-152">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. <span data-ttu-id="9639b-153">Spusťte následující příkaz toocreate trasy v toosend tabulky trasy hello hello všechny přenosy určené toohello front-end podsíť (192.168.1.0/24) toohello **FW1** virtuálních počítačů (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="9639b-153">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. <span data-ttu-id="9639b-154">Spuštění hello následující příkaz tooassociate hello směrovací tabulku s hello **back-end** podsítě:</span><span class="sxs-lookup"><span data-stu-id="9639b-154">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

