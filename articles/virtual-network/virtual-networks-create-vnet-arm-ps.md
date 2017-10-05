---
title: "Vytvoření virtuální sítě - prostředí Azure PowerShell | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť pomocí prostředí PowerShell."
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
ms.openlocfilehash: e7072ddf51570d46578111e2e392e3cbea53f2aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-powershell"></a><span data-ttu-id="29bff-103">Vytvoření virtuální sítě pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="29bff-103">Create a virtual network using PowerShell</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="29bff-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="29bff-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="29bff-105">Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="29bff-105">Microsoft recommends creating resources through the Resource Manager deployment model.</span></span> <span data-ttu-id="29bff-106">Další informace o rozdílech mezi těmito dvěma modely najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="29bff-106">To learn more about the differences between the two models, read the [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="29bff-107">Tento článek vysvětluje, jak vytvořit virtuální síť pomocí modelu nasazení Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="29bff-107">This article explains how to create a VNet through the Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="29bff-108">Virtuální síť můžete vytvořit také prostřednictvím modelu nasazení Resource Manager pomocí jiných nástrojů nebo prostřednictvím modelu nasazení Classic. Pokud to chcete provést, vyberte odpovídající možnost z následujícího seznamu:</span><span class="sxs-lookup"><span data-stu-id="29bff-108">You can also create a VNet through Resource Manager using other tools or create a VNet through the classic deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="29bff-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="29bff-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="29bff-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29bff-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="29bff-111">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="29bff-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="29bff-112">Šablona</span><span class="sxs-lookup"><span data-stu-id="29bff-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="29bff-113">Portál (Classic)</span><span class="sxs-lookup"><span data-stu-id="29bff-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="29bff-114">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="29bff-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="29bff-115">Rozhraní příkazového řádku (Classic)</span><span class="sxs-lookup"><span data-stu-id="29bff-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="29bff-116">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="29bff-116">Create a virtual network</span></span>

<span data-ttu-id="29bff-117">Pokud chcete vytvořit virtuální síť pomocí prostředí PowerShell, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="29bff-117">To create a virtual network using PowerShell, complete the following steps:</span></span>

1. <span data-ttu-id="29bff-118">Instalace a konfigurace prostředí Azure PowerShell, pomocí následujících kroků v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="29bff-118">Install and configure Azure PowerShell, by following the steps in the [How to Install and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>

2. <span data-ttu-id="29bff-119">V případě potřeby vytvořte novou skupinu prostředků, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="29bff-119">If necessary, create a new resource group, as shown below.</span></span> <span data-ttu-id="29bff-120">Pro tento scénář, vytvořte skupinu prostředků s názvem *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="29bff-120">For this scenario, create a resource group named *TestRG*.</span></span> <span data-ttu-id="29bff-121">Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="29bff-121">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="29bff-122">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="29bff-122">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. <span data-ttu-id="29bff-123">Vytvořit novou virtuální síť s názvem *TestVNet*:</span><span class="sxs-lookup"><span data-stu-id="29bff-123">Create a new VNet named *TestVNet*:</span></span>

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    <span data-ttu-id="29bff-124">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="29bff-124">Expected output:</span></span>

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
4. <span data-ttu-id="29bff-125">Uložte objekt virtuální sítě v proměnné:</span><span class="sxs-lookup"><span data-stu-id="29bff-125">Store the virtual network object in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > <span data-ttu-id="29bff-126">Kroky 3 a 4 můžete kombinovat spuštěním `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span><span class="sxs-lookup"><span data-stu-id="29bff-126">You can combine steps 3 and 4 by running `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span></span>
   > 

5. <span data-ttu-id="29bff-127">Přidejte do nové proměnné sítě VNet podsíť:</span><span class="sxs-lookup"><span data-stu-id="29bff-127">Add a subnet to the new VNet variable:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    <span data-ttu-id="29bff-128">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="29bff-128">Expected output:</span></span>
   
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

6. <span data-ttu-id="29bff-129">Výše popsaný krok 5 opakujte pro každou podsíť, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="29bff-129">Repeat step 5 above for each subnet you want to create.</span></span> <span data-ttu-id="29bff-130">Následující příkaz vytvoří *back-end* podsíť pro tento scénář:</span><span class="sxs-lookup"><span data-stu-id="29bff-130">The following command creates the *BackEnd* subnet for the scenario:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. <span data-ttu-id="29bff-131">I když vytvoříte podsítě, zatím existují jenom v místní proměnné sloužící k načtení sítě VNet, kterou vytvoříte ve výše popsaném kroku 4.</span><span class="sxs-lookup"><span data-stu-id="29bff-131">Although you create subnets, they currently only exist in the local variable used to retrieve the VNet you create in step 4 above.</span></span> <span data-ttu-id="29bff-132">Pokud chcete uložit změny do Azure, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="29bff-132">To save the changes to Azure, run the following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    <span data-ttu-id="29bff-133">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="29bff-133">Expected output:</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="29bff-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29bff-134">Next steps</span></span>

<span data-ttu-id="29bff-135">Zjistěte, jak připojit:</span><span class="sxs-lookup"><span data-stu-id="29bff-135">Learn how to connect:</span></span>

- <span data-ttu-id="29bff-136">Virtuální počítač (VM) k virtuální síti načtením [vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-ps-create.md) článku.</span><span class="sxs-lookup"><span data-stu-id="29bff-136">A virtual machine (VM) to a virtual network by reading the [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md) article.</span></span> <span data-ttu-id="29bff-137">Místo vytváření virtuální sítě a podsítě v rámci kroků v těchto článcích můžete vybrat existující virtuální síť a podsíť, ke které se má virtuální počítač připojit.</span><span class="sxs-lookup"><span data-stu-id="29bff-137">Instead of creating a VNet and subnet in the steps of the articles, you can select an existing VNet and subnet to connect a VM to.</span></span>
- <span data-ttu-id="29bff-138">Virtuální síť k jiným virtuálním sítím pomocí informací v článku [Propojení virtuálních sítí](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="29bff-138">The virtual network to other virtual networks by reading the [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="29bff-139">Virtuální síť k místní síti pomocí virtuální privátní sítě (VPN) typu Site-to-Site nebo okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="29bff-139">The virtual network to an on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="29bff-140">Informace najdete v článcích [Připojení virtuální sítě k místní síti pomocí sítě VPN typu Site-to-Site](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [Propojení virtuální sítě s okruhem ExpressRoute](../expressroute/expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="29bff-140">Learn how by reading the [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
