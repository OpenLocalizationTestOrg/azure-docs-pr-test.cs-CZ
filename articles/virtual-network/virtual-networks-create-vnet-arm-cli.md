---
title: "aaaCreate virtuální síť – 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate virtuální sítě pomocí Azure CLI 2.0."
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
ms.openlocfilehash: e79b7fe780fc81f4866f810d830824e43a5a43b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli-20"></a><span data-ttu-id="01bc6-103">Vytvoření virtuální sítě pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="01bc6-103">Create a virtual network using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="01bc6-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="01bc6-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="01bc6-105">Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="01bc6-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="01bc6-106">Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="01bc6-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="01bc6-107">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="01bc6-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="01bc6-108">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="01bc6-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="01bc6-109">[Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely</span><span class="sxs-lookup"><span data-stu-id="01bc6-109">[Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span>
- <span data-ttu-id="01bc6-110">[Azure CLI 2.0](#create-a-virtual-network) -naší nové generace rozhraní příkazového řádku pro model nasazení prostředků správu hello (v tomto článku).</span><span class="sxs-lookup"><span data-stu-id="01bc6-110">[Azure CLI 2.0](#create-a-virtual-network) - our next generation CLI for hello resource management deployment model (this article)\`</span></span>
 
    <span data-ttu-id="01bc6-111">Můžete také vytvořit virtuální síť pomocí Resource Manager pomocí jiných nástrojů nebo vytvoření virtuální sítě pomocí modelu nasazení classic hello výběrem jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="01bc6-111">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="01bc6-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="01bc6-112">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="01bc6-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="01bc6-113">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="01bc6-114">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="01bc6-114">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="01bc6-115">Šablona</span><span class="sxs-lookup"><span data-stu-id="01bc6-115">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="01bc6-116">Portál (Classic)</span><span class="sxs-lookup"><span data-stu-id="01bc6-116">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="01bc6-117">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="01bc6-117">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="01bc6-118">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="01bc6-118">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]


## <a name="create-a-virtual-network"></a><span data-ttu-id="01bc6-119">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="01bc6-119">Create a virtual network</span></span>

<span data-ttu-id="01bc6-120">virtuální síť pomocí toocreate hello Azure CLI 2.0, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01bc6-120">toocreate a virtual network using hello Azure CLI 2.0, complete hello following steps:</span></span>

1. <span data-ttu-id="01bc6-121">Instalace a konfigurace hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="01bc6-121">Install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

2. <span data-ttu-id="01bc6-122">Vytvořte skupinu prostředků pro virtuální síť pomocí hello [vytvořit skupinu az](/cli/azure/group#create) s hello `--name` a `--location` argumenty:</span><span class="sxs-lookup"><span data-stu-id="01bc6-122">Create a resource group for your VNet using hello [az group create](/cli/azure/group#create) command with hello `--name` and `--location` arguments:</span></span>

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. <span data-ttu-id="01bc6-123">Vytvoření virtuální sítě a podsítě:</span><span class="sxs-lookup"><span data-stu-id="01bc6-123">Create a VNet and a subnet:</span></span>

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    <span data-ttu-id="01bc6-124">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="01bc6-124">Expected output:</span></span>
    
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

    <span data-ttu-id="01bc6-125">Použité parametry:</span><span class="sxs-lookup"><span data-stu-id="01bc6-125">Parameters used:</span></span>

    - <span data-ttu-id="01bc6-126">`--name TestVNet`: Název toobe hello virtuální síť vytvořili.</span><span class="sxs-lookup"><span data-stu-id="01bc6-126">`--name TestVNet`: Name of hello VNet toobe created.</span></span>
    - <span data-ttu-id="01bc6-127">`--resource-group TestRG`: # název skupiny prostředků v hello, kterými se řídí hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="01bc6-127">`--resource-group TestRG`: # hello resource group name that controls hello resource.</span></span> 
    - <span data-ttu-id="01bc6-128">`--location centralus`: hello umístění, do které toodeploy.</span><span class="sxs-lookup"><span data-stu-id="01bc6-128">`--location centralus`: hello location into which toodeploy.</span></span>
    - <span data-ttu-id="01bc6-129">`--address-prefix 192.168.0.0/16`: hello předponu adresy a bloku.</span><span class="sxs-lookup"><span data-stu-id="01bc6-129">`--address-prefix 192.168.0.0/16`: hello address prefix and block.</span></span>  
    - <span data-ttu-id="01bc6-130">`--subnet-name FrontEnd`: název hello hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="01bc6-130">`--subnet-name FrontEnd`: hello name of hello subnet.</span></span>
    - <span data-ttu-id="01bc6-131">`--subnet-prefix 192.168.1.0/24`: hello předponu adresy a bloku.</span><span class="sxs-lookup"><span data-stu-id="01bc6-131">`--subnet-prefix 192.168.1.0/24`: hello address prefix and block.</span></span>

    <span data-ttu-id="01bc6-132">toolist hello základní informace toouse v hello vedle příkazů, můžete dotazovat pomocí virtuální sítě hello [filtr dotazu](/cli/azure/query-az-cli2):</span><span class="sxs-lookup"><span data-stu-id="01bc6-132">toolist hello basic information toouse in hello next command, you can query hello VNet using a [query filter](/cli/azure/query-az-cli2):</span></span>

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    <span data-ttu-id="01bc6-133">Produkuje hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="01bc6-133">Which produces hello following output:</span></span>

        Where      Name      Group

        centralus  TestVNet  TestRG

4. <span data-ttu-id="01bc6-134">Vytvoření podsítě:</span><span class="sxs-lookup"><span data-stu-id="01bc6-134">Create a subnet:</span></span>

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    <span data-ttu-id="01bc6-135">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="01bc6-135">Expected output:</span></span>

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

    <span data-ttu-id="01bc6-136">Použité parametry:</span><span class="sxs-lookup"><span data-stu-id="01bc6-136">Parameters used:</span></span>

    - <span data-ttu-id="01bc6-137">`--address-prefix 192.168.2.0/24`: Blok CIDR podsítě.</span><span class="sxs-lookup"><span data-stu-id="01bc6-137">`--address-prefix 192.168.2.0/24`: Subnet CIDR block.</span></span>
    - <span data-ttu-id="01bc6-138">`--name BackEnd`: Název hello novou podsíť.</span><span class="sxs-lookup"><span data-stu-id="01bc6-138">`--name BackEnd`: Name of hello new subnet.</span></span>
    - <span data-ttu-id="01bc6-139">`--resource-group TestRG`: skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="01bc6-139">`--resource-group TestRG`: hello resource group.</span></span>
    - <span data-ttu-id="01bc6-140">`--vnet-name TestVNet`: název hello hello vlastnící virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="01bc6-140">`--vnet-name TestVNet`: hello name of hello owning VNet.</span></span>

5. <span data-ttu-id="01bc6-141">Vlastnosti hello dotazu hello nové virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="01bc6-141">Query hello properties of hello new VNet:</span></span>

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    <span data-ttu-id="01bc6-142">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="01bc6-142">Expected output:</span></span>

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. <span data-ttu-id="01bc6-143">Dotaz hello vlastnosti hello podsítě:</span><span class="sxs-lookup"><span data-stu-id="01bc6-143">Query hello properties of hello subnets:</span></span>

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    <span data-ttu-id="01bc6-144">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="01bc6-144">Expected output:</span></span>

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a><span data-ttu-id="01bc6-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01bc6-145">Next steps</span></span>

<span data-ttu-id="01bc6-146">Zjistěte, jak tooconnect:</span><span class="sxs-lookup"><span data-stu-id="01bc6-146">Learn how tooconnect:</span></span>

- <span data-ttu-id="01bc6-147">Virtuální síť virtuálních počítačů (VM) tooa načtením hello [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-cli.md) článku.</span><span class="sxs-lookup"><span data-stu-id="01bc6-147">A virtual machine (VM) tooa virtual network by reading hello [Create a Linux VM](../virtual-machines/linux/quick-create-cli.md) article.</span></span> <span data-ttu-id="01bc6-148">Místo vytváření virtuálních sítí a podsítí v krocích hello hello článků, můžete vybrat z existující virtuální síť a podsíť tooconnect virtuální počítač, abyste.</span><span class="sxs-lookup"><span data-stu-id="01bc6-148">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="01bc6-149">Hello virtuální sítě tooother virtuální sítě načtením hello [připojení virtuální sítě](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) článku.</span><span class="sxs-lookup"><span data-stu-id="01bc6-149">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) article.</span></span>
- <span data-ttu-id="01bc6-150">Hello virtuální sítě tooan do místní sítě pomocí virtuální privátní sítě site-to-site (VPN) nebo okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="01bc6-150">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="01bc6-151">Zjistěte, jak načtením hello [připojit místní sítě tooan virtuální sítě pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě tooan okruh ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="01bc6-151">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span></span>
