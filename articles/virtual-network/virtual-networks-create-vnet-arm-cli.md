---
title: "Vytvoření virtuální sítě - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť pomocí Azure CLI 2.0."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c7d7b3543f488aedff1ea2c68a2b497e0ca744af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli-20"></a><span data-ttu-id="4dc86-103">Vytvoření virtuální sítě pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4dc86-103">Create a virtual network using the Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="4dc86-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="4dc86-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="4dc86-105">Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4dc86-105">Microsoft recommends creating resources through the Resource Manager deployment model.</span></span> <span data-ttu-id="4dc86-106">Další informace o rozdílech mezi těmito dvěma modely najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4dc86-106">To learn more about the differences between the two models, read the [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="4dc86-107">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="4dc86-107">CLI versions to complete the task</span></span>
<span data-ttu-id="4dc86-108">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="4dc86-108">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="4dc86-109">[Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="4dc86-109">[Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – our CLI for the classic and resource management deployment models</span></span>
- <span data-ttu-id="4dc86-110">[Azure CLI 2.0](#create-a-virtual-network) -naší nové generace rozhraní příkazového řádku pro správu model nasazení prostředku (v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="4dc86-110">[Azure CLI 2.0](#create-a-virtual-network) - our next generation CLI for the resource management deployment model (this article)\`</span></span>
 
    <span data-ttu-id="4dc86-111">Virtuální síť můžete vytvořit také prostřednictvím modelu nasazení Resource Manager pomocí jiných nástrojů nebo prostřednictvím modelu nasazení Classic. Pokud to chcete provést, vyberte odpovídající možnost z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="4dc86-111">You can also create a VNet through Resource Manager using other tools or create a VNet through the classic deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4dc86-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4dc86-112">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="4dc86-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4dc86-113">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="4dc86-114">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4dc86-114">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="4dc86-115">Šablona</span><span class="sxs-lookup"><span data-stu-id="4dc86-115">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="4dc86-116">Portál (Classic)</span><span class="sxs-lookup"><span data-stu-id="4dc86-116">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="4dc86-117">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="4dc86-117">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="4dc86-118">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="4dc86-118">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]


## <a name="create-a-virtual-network"></a><span data-ttu-id="4dc86-119">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="4dc86-119">Create a virtual network</span></span>

<span data-ttu-id="4dc86-120">Pokud chcete vytvořit virtuální síť pomocí Azure CLI 2.0, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4dc86-120">To create a virtual network using the Azure CLI 2.0, complete the following steps:</span></span>

1. <span data-ttu-id="4dc86-121">Instalace a konfigurace nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4dc86-121">Install and configure the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

2. <span data-ttu-id="4dc86-122">Vytvořte skupinu prostředků pro virtuální síť s použitím [vytvořit skupinu az](/cli/azure/group#create) s `--name` a `--location` argumenty:</span><span class="sxs-lookup"><span data-stu-id="4dc86-122">Create a resource group for your VNet using the [az group create](/cli/azure/group#create) command with the `--name` and `--location` arguments:</span></span>

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. <span data-ttu-id="4dc86-123">Vytvoření virtuální sítě a podsítě:</span><span class="sxs-lookup"><span data-stu-id="4dc86-123">Create a VNet and a subnet:</span></span>

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    <span data-ttu-id="4dc86-124">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc86-124">Expected output:</span></span>
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    <span data-ttu-id="4dc86-125">Použité parametry:</span><span class="sxs-lookup"><span data-stu-id="4dc86-125">Parameters used:</span></span>

    - <span data-ttu-id="4dc86-126">`--name TestVNet`: Název sítě VNet, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="4dc86-126">`--name TestVNet`: Name of the VNet to be created.</span></span>
    - <span data-ttu-id="4dc86-127">`--resource-group TestRG`: # Název skupiny prostředků, které řídí prostředku.</span><span class="sxs-lookup"><span data-stu-id="4dc86-127">`--resource-group TestRG`: # The resource group name that controls the resource.</span></span> 
    - <span data-ttu-id="4dc86-128">`--location centralus`: Umístění, do které chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="4dc86-128">`--location centralus`: The location into which to deploy.</span></span>
    - <span data-ttu-id="4dc86-129">`--address-prefix 192.168.0.0/16`: Předponu adresy a bloku.</span><span class="sxs-lookup"><span data-stu-id="4dc86-129">`--address-prefix 192.168.0.0/16`: The address prefix and block.</span></span>  
    - <span data-ttu-id="4dc86-130">`--subnet-name FrontEnd`: Název podsítě.</span><span class="sxs-lookup"><span data-stu-id="4dc86-130">`--subnet-name FrontEnd`: The name of the subnet.</span></span>
    - <span data-ttu-id="4dc86-131">`--subnet-prefix 192.168.1.0/24`: Předponu adresy a bloku.</span><span class="sxs-lookup"><span data-stu-id="4dc86-131">`--subnet-prefix 192.168.1.0/24`: The address prefix and block.</span></span>

    <span data-ttu-id="4dc86-132">Seznam základních informací pro použití v další příkaz, se můžete dotazovat pomocí virtuální sítě [filtr dotazu](/cli/azure/query-az-cli2):</span><span class="sxs-lookup"><span data-stu-id="4dc86-132">To list the basic information to use in the next command, you can query the VNet using a [query filter](/cli/azure/query-az-cli2):</span></span>

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    <span data-ttu-id="4dc86-133">Které vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc86-133">Which produces the following output:</span></span>

        Where      Name      Group

        centralus  TestVNet  TestRG

4. <span data-ttu-id="4dc86-134">Vytvoření podsítě:</span><span class="sxs-lookup"><span data-stu-id="4dc86-134">Create a subnet:</span></span>

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    <span data-ttu-id="4dc86-135">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc86-135">Expected output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    <span data-ttu-id="4dc86-136">Použité parametry:</span><span class="sxs-lookup"><span data-stu-id="4dc86-136">Parameters used:</span></span>

    - <span data-ttu-id="4dc86-137">`--address-prefix 192.168.2.0/24`: Blok CIDR podsítě.</span><span class="sxs-lookup"><span data-stu-id="4dc86-137">`--address-prefix 192.168.2.0/24`: Subnet CIDR block.</span></span>
    - <span data-ttu-id="4dc86-138">`--name BackEnd`: Název nové podsítě.</span><span class="sxs-lookup"><span data-stu-id="4dc86-138">`--name BackEnd`: Name of the new subnet.</span></span>
    - <span data-ttu-id="4dc86-139">`--resource-group TestRG`: Skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="4dc86-139">`--resource-group TestRG`: The resource group.</span></span>
    - <span data-ttu-id="4dc86-140">`--vnet-name TestVNet`: Název vlastnícím virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4dc86-140">`--vnet-name TestVNet`: The name of the owning VNet.</span></span>

5. <span data-ttu-id="4dc86-141">Dotaz na vlastnosti nové sítě VNet:</span><span class="sxs-lookup"><span data-stu-id="4dc86-141">Query the properties of the new VNet:</span></span>

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    <span data-ttu-id="4dc86-142">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc86-142">Expected output:</span></span>

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. <span data-ttu-id="4dc86-143">Dotaz na vlastnosti podsítě:</span><span class="sxs-lookup"><span data-stu-id="4dc86-143">Query the properties of the subnets:</span></span>

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    <span data-ttu-id="4dc86-144">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="4dc86-144">Expected output:</span></span>

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a><span data-ttu-id="4dc86-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4dc86-145">Next steps</span></span>

<span data-ttu-id="4dc86-146">Zjistěte, jak připojit:</span><span class="sxs-lookup"><span data-stu-id="4dc86-146">Learn how to connect:</span></span>

- <span data-ttu-id="4dc86-147">Virtuální počítač (VM) k virtuální síti načtením [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-cli.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4dc86-147">A virtual machine (VM) to a virtual network by reading the [Create a Linux VM](../virtual-machines/linux/quick-create-cli.md) article.</span></span> <span data-ttu-id="4dc86-148">Místo vytváření virtuální sítě a podsítě v rámci kroků v těchto článcích můžete vybrat existující virtuální síť a podsíť, ke které se má virtuální počítač připojit.</span><span class="sxs-lookup"><span data-stu-id="4dc86-148">Instead of creating a VNet and subnet in the steps of the articles, you can select an existing VNet and subnet to connect a VM to.</span></span>
- <span data-ttu-id="4dc86-149">Virtuální síť k jiným virtuálním sítím pomocí informací v článku [Propojení virtuálních sítí](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4dc86-149">The virtual network to other virtual networks by reading the [Connect VNets](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) article.</span></span>
- <span data-ttu-id="4dc86-150">Virtuální síť k místní síti pomocí virtuální privátní sítě (VPN) typu Site-to-Site nebo okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="4dc86-150">The virtual network to an on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="4dc86-151">Zjistěte, jak načtením [připojit virtuální síť k místní síti pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě k okruhu ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4dc86-151">Learn how by reading the [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span></span>