---
title: "virtuální síť pomocí aaaCreate hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální sítě pomocí Azure CLI 1.0 | Správce prostředků."
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
ms.openlocfilehash: f48a8b23a5994164b71c9b51ee8a6810d17f9392
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli"></a><span data-ttu-id="6af77-103">Vytvoření virtuální sítě pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6af77-103">Create a virtual network using hello Azure CLI</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="6af77-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="6af77-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="6af77-105">Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="6af77-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="6af77-106">Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6af77-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="6af77-107">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="6af77-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="6af77-108">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="6af77-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="6af77-109">[Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="6af77-109">[Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) - our next generation CLI for hello resource management deployment model</span></span>
- <span data-ttu-id="6af77-110">[Azure CLI 1.0](#create-a-virtual-network) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="6af77-110">[Azure CLI 1.0](#create-a-virtual-network) – our CLI for hello classic and resource management deployment models (this article)</span></span>

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="6af77-111">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="6af77-111">Create a virtual network</span></span>

<span data-ttu-id="6af77-112">hello toocreate virtuální sítě pomocí rozhraní příkazového řádku Azure, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6af77-112">toocreate a virtual network using hello Azure CLI, complete hello following steps:</span></span>

1. <span data-ttu-id="6af77-113">Instalace a konfigurace rozhraní příkazového řádku Azure podle následující hello kroky hello v hello [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6af77-113">Install and configure hello Azure CLI by following hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md) article.</span></span>

2. <span data-ttu-id="6af77-114">Vytvoření virtuální sítě a podsítě:</span><span class="sxs-lookup"><span data-stu-id="6af77-114">Create a VNet and a subnet:</span></span>

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    <span data-ttu-id="6af77-115">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="6af77-115">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    <span data-ttu-id="6af77-116">Použité parametry:</span><span class="sxs-lookup"><span data-stu-id="6af77-116">Parameters used:</span></span>

   * <span data-ttu-id="6af77-117">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="6af77-117">**--vnet**.</span></span> <span data-ttu-id="6af77-118">Název toobe hello virtuální síť vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6af77-118">Name of hello VNet toobe created.</span></span> <span data-ttu-id="6af77-119">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="6af77-119">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="6af77-120">**-e (nebo--adresní prostor)**.</span><span class="sxs-lookup"><span data-stu-id="6af77-120">**-e (or --address-space)**.</span></span> <span data-ttu-id="6af77-121">Adresní prostor sítě VNet.</span><span class="sxs-lookup"><span data-stu-id="6af77-121">VNet address space.</span></span> <span data-ttu-id="6af77-122">Pro náš scénář *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="6af77-122">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="6af77-123">**-i (nebo - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="6af77-123">**-i (or -cidr)**.</span></span> <span data-ttu-id="6af77-124">Maska sítě ve formátu CIDR.</span><span class="sxs-lookup"><span data-stu-id="6af77-124">Network mask in CIDR format.</span></span> <span data-ttu-id="6af77-125">Pro náš scénář *16*.</span><span class="sxs-lookup"><span data-stu-id="6af77-125">For our scenario, *16*.</span></span>
   * <span data-ttu-id="6af77-126">**-n (nebo--název podsítě**).</span><span class="sxs-lookup"><span data-stu-id="6af77-126">**-n (or --subnet-name**).</span></span> <span data-ttu-id="6af77-127">Název první podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="6af77-127">Name of hello first subnet.</span></span> <span data-ttu-id="6af77-128">V našem scénáři je to *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="6af77-128">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="6af77-129">**-p (nebo--IP adresu podsítě start)**.</span><span class="sxs-lookup"><span data-stu-id="6af77-129">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="6af77-130">Počáteční IP adresa pro podsíť nebo adresního prostoru podsítě.</span><span class="sxs-lookup"><span data-stu-id="6af77-130">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="6af77-131">Pro náš scénář *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="6af77-131">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="6af77-132">**-r (nebo--podsítě cidr)**.</span><span class="sxs-lookup"><span data-stu-id="6af77-132">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="6af77-133">Maska sítě ve formátu CIDR podsítě.</span><span class="sxs-lookup"><span data-stu-id="6af77-133">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="6af77-134">Pro náš scénář *24*.</span><span class="sxs-lookup"><span data-stu-id="6af77-134">For our scenario, *24*.</span></span>
   * <span data-ttu-id="6af77-135">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="6af77-135">**-l (or --location)**.</span></span> <span data-ttu-id="6af77-136">Oblast Azure, kde se má vytvořit hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6af77-136">Azure region where hello VNet is created.</span></span> <span data-ttu-id="6af77-137">Pro náš scénář *střed USA*.</span><span class="sxs-lookup"><span data-stu-id="6af77-137">For our scenario, *Central US*.</span></span>

3. <span data-ttu-id="6af77-138">Vytvoření podsítě:</span><span class="sxs-lookup"><span data-stu-id="6af77-138">Create a subnet:</span></span>

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    <span data-ttu-id="6af77-139">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="6af77-139">Expected output:</span></span>

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    <span data-ttu-id="6af77-140">Použité parametry:</span><span class="sxs-lookup"><span data-stu-id="6af77-140">Parameters used:</span></span>

   * <span data-ttu-id="6af77-141">**-t (nebo--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="6af77-141">**-t (or --vnet-name**.</span></span> <span data-ttu-id="6af77-142">Název hello kde hello se vytvoří podsíť virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6af77-142">Name of hello VNet where hello subnet will be created.</span></span> <span data-ttu-id="6af77-143">V našem scénáři je to *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="6af77-143">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="6af77-144">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="6af77-144">**-n (or --name)**.</span></span> <span data-ttu-id="6af77-145">Název nové podsítě hello.</span><span class="sxs-lookup"><span data-stu-id="6af77-145">Name of hello new subnet.</span></span> <span data-ttu-id="6af77-146">Pro náš scénář *back-end*.</span><span class="sxs-lookup"><span data-stu-id="6af77-146">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="6af77-147">**-a (nebo --address-prefixes)**.</span><span class="sxs-lookup"><span data-stu-id="6af77-147">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="6af77-148">Blok CIDR podsítě.</span><span class="sxs-lookup"><span data-stu-id="6af77-148">Subnet CIDR block.</span></span> <span data-ttu-id="6af77-149">Čtyři našem scénáři *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="6af77-149">Four our scenario, *192.168.2.0/24*.</span></span>
   
4. <span data-ttu-id="6af77-150">Vlastnosti hello tooview hello nové virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="6af77-150">tooview hello properties of hello new VNet:</span></span>

    ```azurecli
    azure network vnet show
    ```
   
    <span data-ttu-id="6af77-151">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="6af77-151">Expected output:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
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

## <a name="next-steps"></a><span data-ttu-id="6af77-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6af77-152">Next steps</span></span>

<span data-ttu-id="6af77-153">Zjistěte, jak tooconnect:</span><span class="sxs-lookup"><span data-stu-id="6af77-153">Learn how tooconnect:</span></span>

- <span data-ttu-id="6af77-154">Virtuální síť virtuálních počítačů (VM) tooa načtením hello [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-cli.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6af77-154">A virtual machine (VM) tooa virtual network by reading hello [Create a Linux VM](../virtual-machines/linux/quick-create-cli.md) article.</span></span> <span data-ttu-id="6af77-155">Místo vytváření virtuálních sítí a podsítí v krocích hello hello článků, můžete vybrat z existující virtuální síť a podsíť tooconnect virtuální počítač, abyste.</span><span class="sxs-lookup"><span data-stu-id="6af77-155">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="6af77-156">Hello virtuální sítě tooother virtuální sítě načtením hello [připojení virtuální sítě](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6af77-156">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) article.</span></span>
- <span data-ttu-id="6af77-157">Hello virtuální sítě tooan do místní sítě pomocí virtuální privátní sítě site-to-site (VPN) nebo okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="6af77-157">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="6af77-158">Zjistěte, jak načtením hello [připojit místní sítě tooan virtuální sítě pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě tooan okruh ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6af77-158">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span></span>
