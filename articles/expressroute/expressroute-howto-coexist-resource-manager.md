---
title: "Konfigurace ExpressRoute a připojení VPN typu site-to-site, která mohou existovat vedle sebe: Resource Manager: Azure | Dokumentace Microsoftu"
description: "Tento článek vás provede konfigurací ExpressRoute a připojení VPN typu site-to-site, která mohou v modelu nasazení Resource Manager existovat vedle sebe."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: charwen,cherylmc
ms.openlocfilehash: b29147a37f9a90fc80e16b350ac9b91daac1d7f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a><span data-ttu-id="091ae-103">Konfigurace společně používaných připojení typu Site-to-Site a ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="091ae-103">Configure ExpressRoute and Site-to-Site coexisting connections</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="091ae-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="091ae-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="091ae-105">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="091ae-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="091ae-106">Konfigurace ExpressRoute a současně existujících připojení VPN typu Site-to-Site má několik výhod.</span><span class="sxs-lookup"><span data-stu-id="091ae-106">Configuring Site-to-Site VPN and ExpressRoute coexisting connections has several advantages.</span></span> <span data-ttu-id="091ae-107">Můžete nakonfigurovat VPN typu Site-to-Site jako zabezpečenou cestu převzetí služeb při selhání pro ExressRoute, nebo použít VPN typu Site-to-Site pro připojení k webům, které nejsou připojené prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="091ae-107">You can configure a Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs to connect to sites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="091ae-108">V tomto článku nabízíme postupy konfigurace pro oba scénáře.</span><span class="sxs-lookup"><span data-stu-id="091ae-108">We cover the steps to configure both scenarios in this article.</span></span> <span data-ttu-id="091ae-109">Tento článek se týká modelu nasazení Resource Manager a používá PowerShell.</span><span class="sxs-lookup"><span data-stu-id="091ae-109">This article applies to the Resource Manager deployment model and uses PowerShell.</span></span> <span data-ttu-id="091ae-110">Tato konfigurace není k dispozici na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="091ae-110">This configuration is not available in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="091ae-111">Než budete postupovat podle dál uvedených pokynů, musí být předem nakonfigurované okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="091ae-111">ExpressRoute circuits must be pre-configured before you follow the instructions below.</span></span> <span data-ttu-id="091ae-112">Než budete pokračovat, zkontrolujte, že jste provedli postupy pro [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a [konfiguraci směrování](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="091ae-112">Make sure that you have followed the guides to [create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [configure routing](expressroute-howto-routing-arm.md) before you proceed.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="091ae-113">Omezení</span><span class="sxs-lookup"><span data-stu-id="091ae-113">Limits and limitations</span></span>
* <span data-ttu-id="091ae-114">**Směrování provozu není podporováno.**</span><span class="sxs-lookup"><span data-stu-id="091ae-114">**Transit routing is not supported.**</span></span> <span data-ttu-id="091ae-115">Nemůžete provádět směrování (přes Azure) mezi místní sítí připojenou prostřednictvím sítě VPN typu site-to-site a místní sítí připojenou přes ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="091ae-115">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="091ae-116">**Základní brána SKU není podporována.**</span><span class="sxs-lookup"><span data-stu-id="091ae-116">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="091ae-117">Pro [bránu ExpressRoute](expressroute-about-virtual-network-gateways.md) a [bránu VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) je nutné použít jinou než základní bránu SKU.</span><span class="sxs-lookup"><span data-stu-id="091ae-117">You must use a non-Basic SKU gateway for both the [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and the [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="091ae-118">**Podporována je pouze brána VPN na základě tras.**</span><span class="sxs-lookup"><span data-stu-id="091ae-118">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="091ae-119">Je nutné použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) na základě tras.</span><span class="sxs-lookup"><span data-stu-id="091ae-119">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="091ae-120">**Pro vaši bránu VPN by měla být nakonfigurována statická trasa.**</span><span class="sxs-lookup"><span data-stu-id="091ae-120">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="091ae-121">Pokud je vaše místní síť připojená k ExpressRoute a síti VPN typu site-to-site, musíte mít v místní síti konfigurovanou statickou trasu, abyste mohli směrovat připojení VPN typu site-to-site do veřejného internetu.</span><span class="sxs-lookup"><span data-stu-id="091ae-121">If your local network is connected to both ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network to route the Site-to-Site VPN connection to the public Internet.</span></span>
* <span data-ttu-id="091ae-122">**Nejprve je potřeba nakonfigurovat bránu ExpressRoute a připojit ji k okruhu.**</span><span class="sxs-lookup"><span data-stu-id="091ae-122">**ExpressRoute gateway must be configured first and linked to a circuit.**</span></span> <span data-ttu-id="091ae-123">Bránu ExpressRoute musíte vytvořit a připojit k okruhu předtím, než přidáte bránu VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="091ae-123">You must create the ExpressRoute gateway first and link it to a circuit before you add the Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="091ae-124">Návrhy konfigurace</span><span class="sxs-lookup"><span data-stu-id="091ae-124">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="091ae-125">Konfigurace VPN typu site-to-site jako cesty převzetí služeb při selhání pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="091ae-125">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="091ae-126">Můžete nakonfigurovat připojení VPN typu site-to-site jako záložní pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="091ae-126">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="091ae-127">To platí jenom pro virtuální sítě, které jsou propojené s cestou soukromého partnerského vztahu Azure.</span><span class="sxs-lookup"><span data-stu-id="091ae-127">This applies only to virtual networks linked to the Azure private peering path.</span></span> <span data-ttu-id="091ae-128">Neexistuje žádné řešení převzetí služeb při selhání založené na VPN pro služby, které jsou přístupné prostřednictvím veřejného partnerského vztahu Azure nebo partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="091ae-128">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="091ae-129">Okruh ExpressRoute je vždy primárním propojením.</span><span class="sxs-lookup"><span data-stu-id="091ae-129">The ExpressRoute circuit is always the primary link.</span></span> <span data-ttu-id="091ae-130">Data prochází cestou VPN typu Site-to-Site jenom v případě, že okruh ExpressRoute selže.</span><span class="sxs-lookup"><span data-stu-id="091ae-130">Data flows through the Site-to-Site VPN path only if the ExpressRoute circuit fails.</span></span>

> [!NOTE]
> <span data-ttu-id="091ae-131">I když v případě, že jsou obě trasy stejné, je okruh ExpressRoute upřednostněný před VPN typu Site-to-Site, Azure k výběru trasy směrem k cíli paketu použije nejdelší shodu předpony.</span><span class="sxs-lookup"><span data-stu-id="091ae-131">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are the same, Azure will use the longest prefix match to choose the route towards the packet's destination.</span></span>
> 
> 

![Současná existence](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a><span data-ttu-id="091ae-133">Konfigurace VPN typu site-to-site pro připojení webů, které nejsou připojené prostřednictvím ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="091ae-133">Configure a Site-to-Site VPN to connect to sites not connected through ExpressRoute</span></span>
<span data-ttu-id="091ae-134">Svoji síť můžete nakonfigurovat tak, že některé weby jsou připojené přímo k Azure prostřednictvím VPN typu site-to-site a některé weby přes ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="091ae-134">You can configure your network where some sites connect directly to Azure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Současná existence](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="091ae-136">Virtuální síť nemůžete nakonfigurovat jako směrovač provozu.</span><span class="sxs-lookup"><span data-stu-id="091ae-136">You cannot configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-the-steps-to-use"></a><span data-ttu-id="091ae-137">Výběr kroků k použití</span><span class="sxs-lookup"><span data-stu-id="091ae-137">Selecting the steps to use</span></span>
<span data-ttu-id="091ae-138">Existují dvě různé sady postupů, ze kterých si můžete vybrat.</span><span class="sxs-lookup"><span data-stu-id="091ae-138">There are two different sets of procedures to choose from.</span></span> <span data-ttu-id="091ae-139">Postup konfigurace, který vyberete, závisí na tom, jestli máte existující virtuální síť, ke které se chcete připojit, nebo chcete vytvořit novou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="091ae-139">The configuration procedure that you select depends on whether you have an existing virtual network that you want to connect to, or you want to create a new virtual network.</span></span>

* <span data-ttu-id="091ae-140">Nemám virtuální síť a potřebuji ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="091ae-140">I don't have a VNet and need to create one.</span></span>
  
    <span data-ttu-id="091ae-141">Pokud ještě nemáte virtuální síť, tento postup vás provede procesem vytvoření nové virtuální sítě pomocí modelu nasazení Resource Manager a vytvoření nových připojení ExpressRoute a VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="091ae-141">If you don’t already have a virtual network, this procedure walks you through creating a new virtual network using Resource Manager deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="091ae-142">Pokud chcete konfigurovat virtuální síť, postupujte podle kroků v části [Vytvoření nové virtuální sítě a současně existujících připojení](#new).</span><span class="sxs-lookup"><span data-stu-id="091ae-142">To configure a virtual network, follow the steps in [To create a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="091ae-143">Už mám virtuální síť modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="091ae-143">I already have a Resource Manager deployment model VNet.</span></span>
  
    <span data-ttu-id="091ae-144">Už můžete mít virtuální síť s existujícím připojením VPN typu site-to-site nebo připojením ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="091ae-144">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="091ae-145">V části [Konfigurace současně existujících připojení pro už existující virtuální síť](#add) najdete postup odstranění brány a následného vytvoření nových připojení ExpressRoute a VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="091ae-145">The [To configure coexisting connections for an already existing VNet](#add) section walks you through deleting the gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="091ae-146">Při vytváření nových připojení musí být kroky provedené ve specifickém pořadí.</span><span class="sxs-lookup"><span data-stu-id="091ae-146">When creating the new connections, the steps must be completed in a specific order.</span></span> <span data-ttu-id="091ae-147">Nepoužívejte pro vytvoření připojení a bran pokyny z jiných článků.</span><span class="sxs-lookup"><span data-stu-id="091ae-147">Don't use the instructions in other articles to create your gateways and connections.</span></span>
  
    <span data-ttu-id="091ae-148">V tomto postupu bude vytvoření připojení, která mohou existovat současně, vyžadovat, abyste odstranili bránu a pak nakonfigurovali nové brány.</span><span class="sxs-lookup"><span data-stu-id="091ae-148">In this procedure, creating connections that can coexist requires you to delete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="091ae-149">Během odstraňování a opětného vytváření brány a připojení budete mít výpadek připojení mezi místy, ale nebude nutné migrovat žádné virtuální počítače ani služby do nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="091ae-149">You will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need to migrate any of your VMs or services to a new virtual network.</span></span> <span data-ttu-id="091ae-150">Virtuální počítače a služby budou během konfigurace brány stále schopné komunikovat prostřednictvím nástroje pro vyrovnávání zatížení, pokud jsou tak nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="091ae-150">Your VMs and services will still be able to communicate out through the load balancer while you configure your gateway if they are configured to do so.</span></span>

## <span data-ttu-id="091ae-151"><a name="new"></a>Vytvoření nové virtuální sítě a současně existujících připojení</span><span class="sxs-lookup"><span data-stu-id="091ae-151"><a name="new"></a>To create a new virtual network and coexisting connections</span></span>
<span data-ttu-id="091ae-152">Tento postup vás provede procesem vytvoření virtuální sítě a připojení ExpressRoute a VPN typu Site-to-Site, která budou existovat společně.</span><span class="sxs-lookup"><span data-stu-id="091ae-152">This procedure walks you through creating a VNet and Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="091ae-153">Nainstalujte nejnovější verzi rutin Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="091ae-153">Install the latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="091ae-154">Informace o instalaci rutin najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="091ae-154">For information about installing the cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="091ae-155">Rutiny, které použijete pro tuto konfiguraci, se můžou mírně lišit od těch, co znáte.</span><span class="sxs-lookup"><span data-stu-id="091ae-155">The cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="091ae-156">Ujistěte se, že používáte rutiny určené v těchto pokynech.</span><span class="sxs-lookup"><span data-stu-id="091ae-156">Be sure to use the cmdlets specified in these instructions.</span></span>
2. <span data-ttu-id="091ae-157">Přihlaste se ke svému účtu a nastavte prostředí.</span><span class="sxs-lookup"><span data-stu-id="091ae-157">Log in to your account and set up the environment.</span></span>

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. <span data-ttu-id="091ae-158">Vytvořte virtuální síť včetně podsítě brány.</span><span class="sxs-lookup"><span data-stu-id="091ae-158">Create a virtual network including Gateway Subnet.</span></span> <span data-ttu-id="091ae-159">Další informace o konfiguraci virtuální sítě najdete v tématu [Konfigurace Azure Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="091ae-159">For more information about the virtual network configuration, see [Azure Virtual Network configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="091ae-160">Podsíť brány musí být /27 nebo kratší předpona (například /26 nebo /25).</span><span class="sxs-lookup"><span data-stu-id="091ae-160">The Gateway Subnet must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   > 
   > 
   
    <span data-ttu-id="091ae-161">Vytvořte novou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="091ae-161">Create a new VNet.</span></span>

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    <span data-ttu-id="091ae-162">Přidejte podsítě.</span><span class="sxs-lookup"><span data-stu-id="091ae-162">Add subnets.</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="091ae-163">Uložte konfiguraci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="091ae-163">Save the VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="091ae-164"><a name="gw"></a>Vytvořte bránu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="091ae-164"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="091ae-165">Další informace o konfiguraci brány ExpressRoute najdete v tématu [Konfigurace brány ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="091ae-165">For more information about the ExpressRoute gateway configuration, see [ExpressRoute gateway configuration](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="091ae-166">GatewaySKU musí být *Standard*, *HighPerformance* nebo *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="091ae-166">The GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. <span data-ttu-id="091ae-167">Propojte bránu ExpressRoute s okruhem ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="091ae-167">Link the ExpressRoute gateway to the ExpressRoute circuit.</span></span> <span data-ttu-id="091ae-168">Po dokončení tohoto kroku bude připojení mezi místní sítí a Azure prostřednictvím ExpressRoute vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="091ae-168">After this step has been completed, the connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span> <span data-ttu-id="091ae-169">Další informace o operaci propojení najdete v tématu [Propojení virtuálních sítí s ExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="091ae-169">For more information about the link operation, see [Link VNets to ExpressRoute](expressroute-howto-linkvnet-arm.md).</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <span data-ttu-id="091ae-170"><a name="vpngw"></a>Dále vytvořte bránu VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="091ae-170"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="091ae-171">Další informace o konfiguraci brány VPN najdete v tématu [Konfigurace virtuální sítě s připojením typu site-to-site](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="091ae-171">For more information about the VPN gateway configuration, see [Configure a VNet with a Site-to-Site connection](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span></span> <span data-ttu-id="091ae-172">GatewaySKU musí být *Standard*, *HighPerformance* nebo *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="091ae-172">The GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span> <span data-ttu-id="091ae-173">VpnType musí být *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="091ae-173">The VpnType must *RouteBased*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    <span data-ttu-id="091ae-174">Brána Azure VPN podporuje směrovací protokol BGP.</span><span class="sxs-lookup"><span data-stu-id="091ae-174">Azure VPN gateway supports BGP routing protocol.</span></span> <span data-ttu-id="091ae-175">Můžete zadat ASN (číslo AS) pro tuto virtuální síť přidáním přepínače -Asn do následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="091ae-175">You can specify ASN (AS Number) for that Virtual Network by adding the -Asn switch in the following command.</span></span> <span data-ttu-id="091ae-176">V případě nezadání parametru se použije výchozí číslo AS 65515.</span><span class="sxs-lookup"><span data-stu-id="091ae-176">Not specifying that parameter will default to AS number 65515.</span></span>

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    <span data-ttu-id="091ae-177">IP adresu partnerských vztahů protokolu BGP a číslo AS, které Azure používá pro bránu VPN, najdete v $azureVpn.BgpSettings.BgpPeeringAddress a $azureVpn.BgpSettings.Asn.</span><span class="sxs-lookup"><span data-stu-id="091ae-177">You can find the BGP peering IP and the AS number that Azure uses for the VPN gateway in $azureVpn.BgpSettings.BgpPeeringAddress and $azureVpn.BgpSettings.Asn.</span></span> <span data-ttu-id="091ae-178">Další informace najdete v tématu [Konfigurace protokolu BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) pro bránu VPN Azure.</span><span class="sxs-lookup"><span data-stu-id="091ae-178">For more information, see [Configure BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) for Azure VPN gateway.</span></span>
7. <span data-ttu-id="091ae-179">Vytvořte entitu brány VPN místního webu.</span><span class="sxs-lookup"><span data-stu-id="091ae-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="091ae-180">Tento příkaz neprovede konfiguraci vaší místní brány VPN.</span><span class="sxs-lookup"><span data-stu-id="091ae-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="091ae-181">Místo toho umožní zadat nastavení místní brány, jako je například veřejná IP adresa a místní adresní prostor, aby se brána Azure VPN k nim mohla připojit.</span><span class="sxs-lookup"><span data-stu-id="091ae-181">Rather, it allows you to provide the local gateway settings, such as the public IP and the on-premises address space, so that the Azure VPN gateway can connect to it.</span></span>
   
    <span data-ttu-id="091ae-182">Pokud vaše místní zařízení VPN podporuje pouze statické směrování, můžete nakonfigurovat statické trasy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="091ae-182">If your local VPN device only supports static routing, you can configure the static routes in the following way:</span></span>

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    <span data-ttu-id="091ae-183">Pokud vaše místní zařízení VPN podporuje protokol BGP a chcete povolit dynamické trasování, potřebujete znát IP adresu partnerských vztahů protokolu BGP a číslo AS, které vaše místní zařízení VPN používá.</span><span class="sxs-lookup"><span data-stu-id="091ae-183">If your local VPN device supports the BGP and you want to enable dynamic routing, you need to know the BGP peering IP and the AS number that your local VPN device uses.</span></span>

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for the BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. <span data-ttu-id="091ae-184">Nakonfigurujte místní zařízení VPN pro připojení k nové bráně Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="091ae-184">Configure your local VPN device to connect to the new Azure VPN gateway.</span></span> <span data-ttu-id="091ae-185">Další informace o konfiguraci zařízení VPN najdete v tématu [Konfigurace zařízení VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="091ae-185">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
9. <span data-ttu-id="091ae-186">Propojte bránu VPN typu site-to-site v Azure s místní bránou.</span><span class="sxs-lookup"><span data-stu-id="091ae-186">Link the Site-to-Site VPN gateway on Azure to the local gateway.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <span data-ttu-id="091ae-187"><a name="add"></a>Konfigurace současně existujících připojení pro už existující virtuální síť</span><span class="sxs-lookup"><span data-stu-id="091ae-187"><a name="add"></a>To configure coexisting connections for an already existing VNet</span></span>
<span data-ttu-id="091ae-188">Pokud máte existující virtuální síť, zkontrolujte velikost podsítě brány.</span><span class="sxs-lookup"><span data-stu-id="091ae-188">If you have an existing virtual network, check the gateway subnet size.</span></span> <span data-ttu-id="091ae-189">Pokud podsíť brány je /28 nebo /29, musíte nejdřív bránu virtuální sítě odstranit a zvýšit velikost podsítě brány.</span><span class="sxs-lookup"><span data-stu-id="091ae-189">If the gateway subnet is /28 or /29, you must first delete the virtual network gateway and increase the gateway subnet size.</span></span> <span data-ttu-id="091ae-190">Postup v této části ukazuje, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="091ae-190">The steps in this section show you how to do that.</span></span>

<span data-ttu-id="091ae-191">Pokud podsíť brány je /27 nebo větší a virtuální síť je připojená přes ExpressRoute, můžete přeskočit následující kroky a přejít ke [kroku 6 – Vytvoření brány VPN typu site-to-site](#vpngw) v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="091ae-191">If the gateway subnet is /27 or larger and the virtual network is connected via ExpressRoute, you can skip the steps below and proceed to ["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in the previous section.</span></span> 

> [!NOTE]
> <span data-ttu-id="091ae-192">Pokud odstraníte existující bránu, místní místo ztratí během práce na této konfiguraci připojení k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="091ae-192">When you delete the existing gateway, your local premises will lose the connection to your virtual network while you are working on this configuration.</span></span> 
> 
> 

1. <span data-ttu-id="091ae-193">Budete potřebovat nainstalovat nejnovější verzi rutin Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="091ae-193">You'll need to install the latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="091ae-194">Další informace o instalaci rutin najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="091ae-194">For more information about installing cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="091ae-195">Rutiny, které použijete pro tuto konfiguraci, se můžou mírně lišit od těch, co znáte.</span><span class="sxs-lookup"><span data-stu-id="091ae-195">The cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="091ae-196">Ujistěte se, že používáte rutiny určené v těchto pokynech.</span><span class="sxs-lookup"><span data-stu-id="091ae-196">Be sure to use the cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="091ae-197">Odstraňte existující bránu ExpressRoute nebo VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="091ae-197">Delete the existing ExpressRoute or Site-to-Site VPN gateway.</span></span>

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. <span data-ttu-id="091ae-198">Odstraňte podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="091ae-198">Delete Gateway Subnet.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="091ae-199">Přidejte podsíť brány, která je /27 nebo větší.</span><span class="sxs-lookup"><span data-stu-id="091ae-199">Add a Gateway Subnet that is /27 or larger.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="091ae-200">Pokud vám ve virtuální síti nezbylo dost IP adres pro zvětšení velikosti podsítě brány, budete muset přidat další adresní prostor IP adres.</span><span class="sxs-lookup"><span data-stu-id="091ae-200">If you don't have enough IP addresses left in your virtual network to increase the gateway subnet size, you need to add more IP address space.</span></span>
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="091ae-201">Uložte konfiguraci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="091ae-201">Save the VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. <span data-ttu-id="091ae-202">V tuto chvíli máte virtuální síť, která nemá žádné brány.</span><span class="sxs-lookup"><span data-stu-id="091ae-202">At this point, you have a VNet with no gateways.</span></span> <span data-ttu-id="091ae-203">Abyste vytvořili nové brány a dokončili připojení, můžete pokračovat [krokem 4 – Vytvoření brány ExpressRoute](#gw), který se nachází v předchozí sadě kroků.</span><span class="sxs-lookup"><span data-stu-id="091ae-203">To create new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in the preceding set of steps.</span></span>

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a><span data-ttu-id="091ae-204">Přidání konfigurace point-to-site k bráně VPN</span><span class="sxs-lookup"><span data-stu-id="091ae-204">To add point-to-site configuration to the VPN gateway</span></span>
<span data-ttu-id="091ae-205">Podle následujících pokynů můžete k bráně VPN v nastavení koexistence přidat konfiguraci point-to-site.</span><span class="sxs-lookup"><span data-stu-id="091ae-205">You can follow the steps below to add Point-to-Site configuration to your VPN gateway in a co-existence setup.</span></span>

1. <span data-ttu-id="091ae-206">Přidejte fond adres klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="091ae-206">Add VPN Client address pool.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. <span data-ttu-id="091ae-207">Odešlete kořenový certifikát VPN pro bránu VPN do Azure.</span><span class="sxs-lookup"><span data-stu-id="091ae-207">Upload the VPN root certificate to Azure for your VPN gateway.</span></span> <span data-ttu-id="091ae-208">V tomto příkladu se předpokládá, že kořenový certifikát je uložený v místním počítači, kde se spustí následující rutiny PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="091ae-208">In this example, it's assumed that the root certificate is stored in the local machine where the following PowerShell cmdlets are run.</span></span>

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

<span data-ttu-id="091ae-209">Další informace o VPN typu point-to-site najdete v tématu [Konfigurace připojení typu point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="091ae-209">For more information on Point-to-Site VPN, see [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="091ae-210">Další kroky</span><span class="sxs-lookup"><span data-stu-id="091ae-210">Next steps</span></span>
<span data-ttu-id="091ae-211">Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="091ae-211">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
