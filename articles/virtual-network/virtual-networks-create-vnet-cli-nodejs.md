---
title: "Vytvoření virtuální sítě pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť pomocí Azure CLI 1.0 | Správce prostředků."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f0649c5c8c04dda72d2f147601efb37217f9bade
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a><span data-ttu-id="b76ba-103">Vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b76ba-103">Create a virtual network using the Azure CLI</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="b76ba-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="b76ba-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="b76ba-105">Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b76ba-105">Microsoft recommends creating resources through the Resource Manager deployment model.</span></span> <span data-ttu-id="b76ba-106">Další informace o rozdílech mezi těmito dvěma modely najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b76ba-106">To learn more about the differences between the two models, read the [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="b76ba-107">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="b76ba-107">CLI versions to complete the task</span></span>
<span data-ttu-id="b76ba-108">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="b76ba-108">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="b76ba-109">[Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="b76ba-109">[Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) - our next generation CLI for the resource management deployment model</span></span>
- <span data-ttu-id="b76ba-110">[Azure CLI 1.0](#create-a-virtual-network) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="b76ba-110">[Azure CLI 1.0](#create-a-virtual-network) – our CLI for the classic and resource management deployment models (this article)</span></span>

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="b76ba-111">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="b76ba-111">Create a virtual network</span></span>

<span data-ttu-id="b76ba-112">Pokud chcete vytvořit virtuální síť pomocí rozhraní příkazového řádku Azure, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b76ba-112">To create a virtual network using the Azure CLI, complete the following steps:</span></span>

1. <span data-ttu-id="b76ba-113">Instalace a konfigurace rozhraní příkazového řádku Azure pomocí následujících kroků v [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b76ba-113">Install and configure the Azure CLI by following the steps in the [Install and Configure the Azure CLI](../cli-install-nodejs.md) article.</span></span>

2. <span data-ttu-id="b76ba-114">Vytvoření virtuální sítě a podsítě:</span><span class="sxs-lookup"><span data-stu-id="b76ba-114">Create a VNet and a subnet:</span></span>

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    <span data-ttu-id="b76ba-115">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="b76ba-115">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    <span data-ttu-id="b76ba-116">Použité parametry:</span><span class="sxs-lookup"><span data-stu-id="b76ba-116">Parameters used:</span></span>

   * <span data-ttu-id="b76ba-117">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-117">**--vnet**.</span></span> <span data-ttu-id="b76ba-118">Název sítě VNet, která se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b76ba-118">Name of the VNet to be created.</span></span> <span data-ttu-id="b76ba-119">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-119">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="b76ba-120">**-e (nebo--adresní prostor)**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-120">**-e (or --address-space)**.</span></span> <span data-ttu-id="b76ba-121">Adresní prostor sítě VNet.</span><span class="sxs-lookup"><span data-stu-id="b76ba-121">VNet address space.</span></span> <span data-ttu-id="b76ba-122">Pro náš scénář *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="b76ba-122">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="b76ba-123">**-i (nebo - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-123">**-i (or -cidr)**.</span></span> <span data-ttu-id="b76ba-124">Maska sítě ve formátu CIDR.</span><span class="sxs-lookup"><span data-stu-id="b76ba-124">Network mask in CIDR format.</span></span> <span data-ttu-id="b76ba-125">Pro náš scénář *16*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-125">For our scenario, *16*.</span></span>
   * <span data-ttu-id="b76ba-126">**-n (nebo--název podsítě**).</span><span class="sxs-lookup"><span data-stu-id="b76ba-126">**-n (or --subnet-name**).</span></span> <span data-ttu-id="b76ba-127">Název první podsíť.</span><span class="sxs-lookup"><span data-stu-id="b76ba-127">Name of the first subnet.</span></span> <span data-ttu-id="b76ba-128">V našem scénáři je to *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-128">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="b76ba-129">**-p (nebo--IP adresu podsítě start)**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-129">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="b76ba-130">Počáteční IP adresa pro podsíť nebo adresního prostoru podsítě.</span><span class="sxs-lookup"><span data-stu-id="b76ba-130">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="b76ba-131">Pro náš scénář *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-131">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="b76ba-132">**-r (nebo--podsítě cidr)**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-132">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="b76ba-133">Maska sítě ve formátu CIDR podsítě.</span><span class="sxs-lookup"><span data-stu-id="b76ba-133">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="b76ba-134">Pro náš scénář *24*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-134">For our scenario, *24*.</span></span>
   * <span data-ttu-id="b76ba-135">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-135">**-l (or --location)**.</span></span> <span data-ttu-id="b76ba-136">Oblast Azure, kde se má vytvořit síť VNet.</span><span class="sxs-lookup"><span data-stu-id="b76ba-136">Azure region where the VNet is created.</span></span> <span data-ttu-id="b76ba-137">Pro náš scénář *střed USA*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-137">For our scenario, *Central US*.</span></span>

3. <span data-ttu-id="b76ba-138">Vytvoření podsítě:</span><span class="sxs-lookup"><span data-stu-id="b76ba-138">Create a subnet:</span></span>

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    <span data-ttu-id="b76ba-139">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="b76ba-139">Expected output:</span></span>

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    <span data-ttu-id="b76ba-140">Použité parametry:</span><span class="sxs-lookup"><span data-stu-id="b76ba-140">Parameters used:</span></span>

   * <span data-ttu-id="b76ba-141">**-t (nebo--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-141">**-t (or --vnet-name**.</span></span> <span data-ttu-id="b76ba-142">Název sítě VNet, ve které se vytvoří podsíť.</span><span class="sxs-lookup"><span data-stu-id="b76ba-142">Name of the VNet where the subnet will be created.</span></span> <span data-ttu-id="b76ba-143">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-143">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="b76ba-144">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-144">**-n (or --name)**.</span></span> <span data-ttu-id="b76ba-145">Název nové podsítě.</span><span class="sxs-lookup"><span data-stu-id="b76ba-145">Name of the new subnet.</span></span> <span data-ttu-id="b76ba-146">Pro náš scénář *back-end*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-146">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="b76ba-147">**-a (nebo --address-prefixes)**.</span><span class="sxs-lookup"><span data-stu-id="b76ba-147">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="b76ba-148">Blok CIDR podsítě.</span><span class="sxs-lookup"><span data-stu-id="b76ba-148">Subnet CIDR block.</span></span> <span data-ttu-id="b76ba-149">Čtyři našem scénáři *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="b76ba-149">Four our scenario, *192.168.2.0/24*.</span></span>
   
4. <span data-ttu-id="b76ba-150">Chcete-li zobrazit vlastnosti nové sítě VNet:</span><span class="sxs-lookup"><span data-stu-id="b76ba-150">To view the properties of the new VNet:</span></span>

    ```azurecli
    azure network vnet show
    ```
   
    <span data-ttu-id="b76ba-151">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="b76ba-151">Expected output:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a><span data-ttu-id="b76ba-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b76ba-152">Next steps</span></span>

<span data-ttu-id="b76ba-153">Zjistěte, jak připojit:</span><span class="sxs-lookup"><span data-stu-id="b76ba-153">Learn how to connect:</span></span>

- <span data-ttu-id="b76ba-154">Virtuální počítač (VM) k virtuální síti načtením [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-cli.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b76ba-154">A virtual machine (VM) to a virtual network by reading the [Create a Linux VM](../virtual-machines/linux/quick-create-cli.md) article.</span></span> <span data-ttu-id="b76ba-155">Místo vytváření virtuální sítě a podsítě v rámci kroků v těchto článcích můžete vybrat existující virtuální síť a podsíť, ke které se má virtuální počítač připojit.</span><span class="sxs-lookup"><span data-stu-id="b76ba-155">Instead of creating a VNet and subnet in the steps of the articles, you can select an existing VNet and subnet to connect a VM to.</span></span>
- <span data-ttu-id="b76ba-156">Virtuální síť k jiným virtuálním sítím pomocí informací v článku [Propojení virtuálních sítí](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b76ba-156">The virtual network to other virtual networks by reading the [Connect VNets](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) article.</span></span>
- <span data-ttu-id="b76ba-157">Virtuální síť k místní síti pomocí virtuální privátní sítě (VPN) typu Site-to-Site nebo okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b76ba-157">The virtual network to an on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="b76ba-158">Zjistěte, jak načtením [připojit virtuální síť k místní síti pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě k okruhu ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b76ba-158">Learn how by reading the [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span></span>