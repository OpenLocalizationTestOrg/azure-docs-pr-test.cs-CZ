---
title: "aaaCreate virtuální síť – Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate a virtuální sítě pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a31f4f12-54ee-4339-b968-1a8097ca77d3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d6e395a77f71de9f94b6304b05450e46b47544f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-powershell"></a><span data-ttu-id="d5a6e-103">Vytvoření virtuální sítě pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5a6e-103">Create a virtual network using PowerShell</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="d5a6e-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="d5a6e-105">Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="d5a6e-106">Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="d5a6e-107">Tento článek vysvětluje, jak toocreate virtuální síť prostřednictvím nasazení Resource Manager hello modelu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-107">This article explains how toocreate a VNet through hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="d5a6e-108">Můžete také vytvořit virtuální síť pomocí Resource Manager pomocí jiných nástrojů nebo vytvoření virtuální sítě pomocí modelu nasazení classic hello výběrem jinou možnost z hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-108">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d5a6e-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d5a6e-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="d5a6e-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5a6e-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="d5a6e-111">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="d5a6e-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="d5a6e-112">Šablona</span><span class="sxs-lookup"><span data-stu-id="d5a6e-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="d5a6e-113">Portál (Classic)</span><span class="sxs-lookup"><span data-stu-id="d5a6e-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="d5a6e-114">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="d5a6e-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="d5a6e-115">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="d5a6e-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="d5a6e-116">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d5a6e-116">Create a virtual network</span></span>

<span data-ttu-id="d5a6e-117">toocreate, které virtuální sítě pomocí prostředí PowerShell, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-117">toocreate a virtual network using PowerShell, complete hello following steps:</span></span>

1. <span data-ttu-id="d5a6e-118">Instalace a konfigurace prostředí Azure PowerShell, pomocí následujících kroků hello v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-118">Install and configure Azure PowerShell, by following hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>

2. <span data-ttu-id="d5a6e-119">V případě potřeby vytvořte novou skupinu prostředků, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-119">If necessary, create a new resource group, as shown below.</span></span> <span data-ttu-id="d5a6e-120">Pro tento scénář, vytvořte skupinu prostředků s názvem *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-120">For this scenario, create a resource group named *TestRG*.</span></span> <span data-ttu-id="d5a6e-121">Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5a6e-121">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="d5a6e-122">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-122">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. <span data-ttu-id="d5a6e-123">Vytvořit novou virtuální síť s názvem *TestVNet*:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-123">Create a new VNet named *TestVNet*:</span></span>

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    <span data-ttu-id="d5a6e-124">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-124">Expected output:</span></span>

        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                   : W/"[Id]"
        ProvisioningState          : Succeeded
        Tags                       : 
        AddressSpace               : {
                                   "AddressPrefixes": [
                                     "192.168.0.0/16"
                                   ]
                                  }
        DhcpOptions                : {}
        Subnets                    : []
        VirtualNetworkPeerings     : []
4. <span data-ttu-id="d5a6e-125">Uložte objekt virtuální sítě hello v proměnné:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-125">Store hello virtual network object in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > <span data-ttu-id="d5a6e-126">Kroky 3 a 4 můžete kombinovat spuštěním `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-126">You can combine steps 3 and 4 by running `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span></span>
   > 

5. <span data-ttu-id="d5a6e-127">Přidáte nové sítě VNet proměnné toohello podsítě:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-127">Add a subnet toohello new VNet variable:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    <span data-ttu-id="d5a6e-128">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-128">Expected output:</span></span>
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {}
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24"
                                  }
                                ]
        VirtualNetworkPeerings     : []

6. <span data-ttu-id="d5a6e-129">Opakujte krok 5 výše pro každou podsíť chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-129">Repeat step 5 above for each subnet you want toocreate.</span></span> <span data-ttu-id="d5a6e-130">Hello následující příkaz vytvoří hello *back-end* podsíť pro scénář hello:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-130">hello following command creates hello *BackEnd* subnet for hello scenario:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. <span data-ttu-id="d5a6e-131">I když vytvoříte podsítě, zatím existují jenom v místní proměnné používané tooretrieve hello hello virtuální sítě, které vytvoříte v kroku 4 výše.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-131">Although you create subnets, they currently only exist in hello local variable used tooretrieve hello VNet you create in step 4 above.</span></span> <span data-ttu-id="d5a6e-132">toosave hello změny tooAzure, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-132">toosave hello changes tooAzure, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    <span data-ttu-id="d5a6e-133">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-133">Expected output:</span></span>
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {
                                  "DnsServers": []
                                }
        Subnets               : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []

## <a name="next-steps"></a><span data-ttu-id="d5a6e-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5a6e-134">Next steps</span></span>

<span data-ttu-id="d5a6e-135">Zjistěte, jak tooconnect:</span><span class="sxs-lookup"><span data-stu-id="d5a6e-135">Learn how tooconnect:</span></span>

- <span data-ttu-id="d5a6e-136">Virtuální síť virtuálních počítačů (VM) tooa načtením hello [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md) článku.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-136">A virtual machine (VM) tooa virtual network by reading hello [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md) article.</span></span> <span data-ttu-id="d5a6e-137">Místo vytváření virtuálních sítí a podsítí v krocích hello hello článků, můžete vybrat z existující virtuální síť a podsíť tooconnect virtuální počítač, abyste.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-137">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="d5a6e-138">Hello virtuální sítě tooother virtuální sítě načtením hello [připojení virtuální sítě](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) článku.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-138">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="d5a6e-139">Hello virtuální sítě tooan do místní sítě pomocí virtuální privátní sítě site-to-site (VPN) nebo okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-139">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="d5a6e-140">Zjistěte, jak načtením hello [připojit místní sítě tooan virtuální sítě pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě tooan okruh ExpressRoute](../expressroute/expressroute-howto-linkvnet-arm.md) články.</span><span class="sxs-lookup"><span data-stu-id="d5a6e-140">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
