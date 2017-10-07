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
ms.openlocfilehash: efda9f89d95617c8c4e75af91b20631dc468d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a><span data-ttu-id="ceb1f-103">Konfigurace společně používaných připojení typu Site-to-Site a ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="ceb1f-103">Configure ExpressRoute and Site-to-Site coexisting connections</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ceb1f-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ceb1f-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="ceb1f-105">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="ceb1f-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="ceb1f-106">Konfigurace ExpressRoute a současně existujících připojení VPN typu Site-to-Site má několik výhod.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-106">Configuring Site-to-Site VPN and ExpressRoute coexisting connections has several advantages.</span></span> <span data-ttu-id="ceb1f-107">Můžete nakonfigurovat VPN typu Site-to-Site jako cestu zabezpečené převzetí služeb při selhání pro ExressRoute, nebo použít toosites tooconnect sítě Site-to-Site VPN, které nejsou připojené prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-107">You can configure a Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs tooconnect toosites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="ceb1f-108">Nabídneme tooconfigure kroky hello oba scénáře v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-108">We cover hello steps tooconfigure both scenarios in this article.</span></span> <span data-ttu-id="ceb1f-109">Tento článek vztahuje toohello modelu nasazení Resource Manager a používá prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-109">This article applies toohello Resource Manager deployment model and uses PowerShell.</span></span> <span data-ttu-id="ceb1f-110">Tato konfigurace není k dispozici v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-110">This configuration is not available in hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ceb1f-111">Okruhy ExpressRoute musí být předem nakonfigurované, než začnete postupovat podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-111">ExpressRoute circuits must be pre-configured before you follow hello instructions below.</span></span> <span data-ttu-id="ceb1f-112">Ujistěte se, že jste postupovali podle příručky hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a [konfigurace směrování](expressroute-howto-routing-arm.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-112">Make sure that you have followed hello guides too[create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [configure routing](expressroute-howto-routing-arm.md) before you proceed.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="ceb1f-113">Omezení</span><span class="sxs-lookup"><span data-stu-id="ceb1f-113">Limits and limitations</span></span>
* <span data-ttu-id="ceb1f-114">**Směrování provozu není podporováno.**</span><span class="sxs-lookup"><span data-stu-id="ceb1f-114">**Transit routing is not supported.**</span></span> <span data-ttu-id="ceb1f-115">Nemůžete provádět směrování (přes Azure) mezi místní sítí připojenou prostřednictvím sítě VPN typu site-to-site a místní sítí připojenou přes ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-115">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="ceb1f-116">**Základní brána SKU není podporována.**</span><span class="sxs-lookup"><span data-stu-id="ceb1f-116">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="ceb1f-117">Musíte použít bránu bez – základní SKU pro obě hello [brány ExpressRoute](expressroute-about-virtual-network-gateways.md) a hello [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-117">You must use a non-Basic SKU gateway for both hello [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and hello [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="ceb1f-118">**Podporována je pouze brána VPN na základě tras.**</span><span class="sxs-lookup"><span data-stu-id="ceb1f-118">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="ceb1f-119">Je nutné použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) na základě tras.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-119">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="ceb1f-120">**Pro vaši bránu VPN by měla být nakonfigurována statická trasa.**</span><span class="sxs-lookup"><span data-stu-id="ceb1f-120">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="ceb1f-121">Pokud vaše místní síť připojená tooboth ExpressRoute a Site-to-Site VPN, musíte mít ve vaší místní síti tooroute hello Site-to-Site VPN připojení toohello konfigurovanou statickou trasu veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-121">If your local network is connected tooboth ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network tooroute hello Site-to-Site VPN connection toohello public Internet.</span></span>
* <span data-ttu-id="ceb1f-122">**Bránu ExpressRoute musí být nakonfigurovaná a propojit tooa okruh.**</span><span class="sxs-lookup"><span data-stu-id="ceb1f-122">**ExpressRoute gateway must be configured first and linked tooa circuit.**</span></span> <span data-ttu-id="ceb1f-123">Je nutné nejprve vytvořte bránu ExpressRoute hello a tu propojit tooa okruhu předtím, než přidáte bránu VPN hello Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-123">You must create hello ExpressRoute gateway first and link it tooa circuit before you add hello Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="ceb1f-124">Návrhy konfigurace</span><span class="sxs-lookup"><span data-stu-id="ceb1f-124">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="ceb1f-125">Konfigurace VPN typu site-to-site jako cesty převzetí služeb při selhání pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="ceb1f-125">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="ceb1f-126">Můžete nakonfigurovat připojení VPN typu site-to-site jako záložní pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-126">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="ceb1f-127">To platí pouze toovirtual sítě propojené toohello cestou soukromého partnerského vztahu Azure.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-127">This applies only toovirtual networks linked toohello Azure private peering path.</span></span> <span data-ttu-id="ceb1f-128">Neexistuje žádné řešení převzetí služeb při selhání založené na VPN pro služby, které jsou přístupné prostřednictvím veřejného partnerského vztahu Azure nebo partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-128">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="ceb1f-129">Hello okruh ExpressRoute je vždy hello primární propojení.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-129">hello ExpressRoute circuit is always hello primary link.</span></span> <span data-ttu-id="ceb1f-130">Data proudí prostřednictvím cesty hello Site-to-Site VPN, pouze pokud hello okruh ExpressRoute selže.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-130">Data flows through hello Site-to-Site VPN path only if hello ExpressRoute circuit fails.</span></span>

> [!NOTE]
> <span data-ttu-id="ceb1f-131">Při okruh ExpressRoute je upřednostňované prostřednictvím sítě Site-to-Site VPN, když oba trasy jsou hello stejné, použije Azure hello nejdelší předponu shodu toochoose hello trasy směrem cíle hello paketu.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-131">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are hello same, Azure will use hello longest prefix match toochoose hello route towards hello packet's destination.</span></span>
> 
> 

![Současná existence](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a><span data-ttu-id="ceb1f-133">Konfigurace Site-to-Site VPN tooconnect toosites nejsou připojené prostřednictvím ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="ceb1f-133">Configure a Site-to-Site VPN tooconnect toosites not connected through ExpressRoute</span></span>
<span data-ttu-id="ceb1f-134">Můžete nakonfigurovat síti, kde některé weby jsou připojené přímo tooAzure prostřednictvím sítě Site-to-Site VPN a některé weby přes ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-134">You can configure your network where some sites connect directly tooAzure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Současná existence](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="ceb1f-136">Virtuální síť nemůžete nakonfigurovat jako směrovač provozu.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-136">You cannot configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-hello-steps-toouse"></a><span data-ttu-id="ceb1f-137">Výběr toouse kroky hello</span><span class="sxs-lookup"><span data-stu-id="ceb1f-137">Selecting hello steps toouse</span></span>
<span data-ttu-id="ceb1f-138">Existují dvě sady postupů toochoose z.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-138">There are two different sets of procedures toochoose from.</span></span> <span data-ttu-id="ceb1f-139">Postup konfigurace Hello, který vyberete, závisí na tom, jestli máte existující virtuální síť, které mají tooconnect k, nebo chcete toocreate nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-139">hello configuration procedure that you select depends on whether you have an existing virtual network that you want tooconnect to, or you want toocreate a new virtual network.</span></span>

* <span data-ttu-id="ceb1f-140">Nemám virtuální síť a potřebuji toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-140">I don't have a VNet and need toocreate one.</span></span>
  
    <span data-ttu-id="ceb1f-141">Pokud ještě nemáte virtuální síť, tento postup vás provede procesem vytvoření nové virtuální sítě pomocí modelu nasazení Resource Manager a vytvoření nových připojení ExpressRoute a VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-141">If you don’t already have a virtual network, this procedure walks you through creating a new virtual network using Resource Manager deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="ceb1f-142">tooconfigure virtuální sítě, postupujte podle kroků hello v [toocreate nové virtuální sítě a koexistujících připojení](#new).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-142">tooconfigure a virtual network, follow hello steps in [toocreate a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="ceb1f-143">Už mám virtuální síť modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-143">I already have a Resource Manager deployment model VNet.</span></span>
  
    <span data-ttu-id="ceb1f-144">Už můžete mít virtuální síť s existujícím připojením VPN typu site-to-site nebo připojením ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-144">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="ceb1f-145">Hello [tooconfigure koexistujících připojení pro už existující virtuální síť](#add) části najdete postup odstranění hello brány a následného vytvoření nových připojení ExpressRoute a Site-to-Site VPN.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-145">hello [tooconfigure coexisting connections for an already existing VNet](#add) section walks you through deleting hello gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="ceb1f-146">Při vytváření nové připojení hello hello kroky musí dokončit v určitém pořadí.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-146">When creating hello new connections, hello steps must be completed in a specific order.</span></span> <span data-ttu-id="ceb1f-147">Nepoužívejte hello pokyny v jiných článcích toocreate připojení a bran.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-147">Don't use hello instructions in other articles toocreate your gateways and connections.</span></span>
  
    <span data-ttu-id="ceb1f-148">V tomto postupu vytvoření připojení, která mohou existovat vedle sebe vyžaduje toodelete můžete bránu a pak nakonfigurovali nové brány.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-148">In this procedure, creating connections that can coexist requires you toodelete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="ceb1f-149">Budete mít výpadku pro připojení mezi různými místy odstranit a znovu vytvořte brány a připojení, ale nebude nutné toomigrate všechny virtuální počítače a služby tooa nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-149">You will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need toomigrate any of your VMs or services tooa new virtual network.</span></span> <span data-ttu-id="ceb1f-150">Virtuální počítače a služby bude nadále možné toocommunicate se prostřednictvím nástroje pro vyrovnávání zatížení hello během konfigurace brány, pokud jsou nakonfigurované toodo tak.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-150">Your VMs and services will still be able toocommunicate out through hello load balancer while you configure your gateway if they are configured toodo so.</span></span>

## <span data-ttu-id="ceb1f-151"><a name="new"></a>toocreate nové virtuální sítě a koexistujících připojení</span><span class="sxs-lookup"><span data-stu-id="ceb1f-151"><a name="new"></a>toocreate a new virtual network and coexisting connections</span></span>
<span data-ttu-id="ceb1f-152">Tento postup vás provede procesem vytvoření virtuální sítě a připojení ExpressRoute a VPN typu Site-to-Site, která budou existovat společně.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-152">This procedure walks you through creating a VNet and Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="ceb1f-153">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-153">Install hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="ceb1f-154">Informace o instalaci rutin hello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-154">For information about installing hello cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="ceb1f-155">Hello rutin, které používáte pro tuto konfiguraci můžou mírně lišit, než co je znají.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-155">hello cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="ceb1f-156">Být jisti toouse hello rutiny určené v těchto pokynech.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-156">Be sure toouse hello cmdlets specified in these instructions.</span></span>
2. <span data-ttu-id="ceb1f-157">Přihlaste se v účtu tooyour a nastavení prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-157">Log in tooyour account and set up hello environment.</span></span>

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. <span data-ttu-id="ceb1f-158">Vytvořte virtuální síť včetně podsítě brány.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-158">Create a virtual network including Gateway Subnet.</span></span> <span data-ttu-id="ceb1f-159">Další informace o konfiguraci virtuální sítě hello najdete v tématu [konfigurace Azure Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-159">For more information about hello virtual network configuration, see [Azure Virtual Network configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="ceb1f-160">Hello podsíť brány musí být/27 nebo kratší předpona (například/26 nebo /25).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-160">hello Gateway Subnet must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   > 
   > 
   
    <span data-ttu-id="ceb1f-161">Vytvořte novou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-161">Create a new VNet.</span></span>

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    <span data-ttu-id="ceb1f-162">Přidejte podsítě.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-162">Add subnets.</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="ceb1f-163">Uložte konfiguraci virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-163">Save hello VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="ceb1f-164"><a name="gw"></a>Vytvořte bránu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-164"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="ceb1f-165">Další informace o konfiguraci brány ExpressRoute hello najdete v tématu [konfigurace brány ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-165">For more information about hello ExpressRoute gateway configuration, see [ExpressRoute gateway configuration](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="ceb1f-166">Hello GatewaySKU musí být *standardní*, *HighPerformance*, nebo *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-166">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. <span data-ttu-id="ceb1f-167">Propojení okruhu ExpressRoute toohello brány ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-167">Link hello ExpressRoute gateway toohello ExpressRoute circuit.</span></span> <span data-ttu-id="ceb1f-168">Po dokončení tohoto kroku se hello připojení mezi místní sítí a Azure prostřednictvím ExpressRoute vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-168">After this step has been completed, hello connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span> <span data-ttu-id="ceb1f-169">Další informace o operaci propojení hello najdete v tématu [propojení virtuálních sítí tooExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-169">For more information about hello link operation, see [Link VNets tooExpressRoute](expressroute-howto-linkvnet-arm.md).</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <span data-ttu-id="ceb1f-170"><a name="vpngw"></a>Dále vytvořte bránu VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-170"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="ceb1f-171">Další informace o konfiguraci brány VPN hello najdete v tématu [konfigurace virtuální sítě pomocí připojení Site-to-Site](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-171">For more information about hello VPN gateway configuration, see [Configure a VNet with a Site-to-Site connection](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span></span> <span data-ttu-id="ceb1f-172">Hello GatewaySKU musí být *standardní*, *HighPerformance*, nebo *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-172">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span> <span data-ttu-id="ceb1f-173">Hello VpnType musí *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-173">hello VpnType must *RouteBased*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    <span data-ttu-id="ceb1f-174">Brána Azure VPN podporuje směrovací protokol BGP.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-174">Azure VPN gateway supports BGP routing protocol.</span></span> <span data-ttu-id="ceb1f-175">Přidáte přepínač - Asn hello hello následující příkaz, můžete zadat číslo ASN (čísla AS) pro tuto virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-175">You can specify ASN (AS Number) for that Virtual Network by adding hello -Asn switch in hello following command.</span></span> <span data-ttu-id="ceb1f-176">Není zadáním tohoto parametru bude tooAS výchozí číslo 65515.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-176">Not specifying that parameter will default tooAS number 65515.</span></span>

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    <span data-ttu-id="ceb1f-177">Můžete najít hello IP partnerského vztahu protokolu BGP a hello jako číslo, které Azure používá pro bránu VPN hello v $azureVpn.BgpSettings.BgpPeeringAddress a $azureVpn.BgpSettings.Asn.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-177">You can find hello BGP peering IP and hello AS number that Azure uses for hello VPN gateway in $azureVpn.BgpSettings.BgpPeeringAddress and $azureVpn.BgpSettings.Asn.</span></span> <span data-ttu-id="ceb1f-178">Další informace najdete v tématu [Konfigurace protokolu BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) pro bránu VPN Azure.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-178">For more information, see [Configure BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) for Azure VPN gateway.</span></span>
7. <span data-ttu-id="ceb1f-179">Vytvořte entitu brány VPN místního webu.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="ceb1f-180">Tento příkaz neprovede konfiguraci vaší místní brány VPN.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="ceb1f-181">Místo toho můžete nastavení místní brány hello tooprovide, jako je například veřejná IP adresa hello a hello místní adresní prostor, aby hello Azure VPN gateway bude moct připojit tooit.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-181">Rather, it allows you tooprovide hello local gateway settings, such as hello public IP and hello on-premises address space, so that hello Azure VPN gateway can connect tooit.</span></span>
   
    <span data-ttu-id="ceb1f-182">Pokud vaše místní zařízení VPN podporuje jenom statické směrování, můžete nakonfigurovat statické trasy hello v hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ceb1f-182">If your local VPN device only supports static routing, you can configure hello static routes in hello following way:</span></span>

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    <span data-ttu-id="ceb1f-183">Pokud vaše místní zařízení VPN podporuje hello protokolu BGP a chcete tooenable dynamické směrování, je třeba tooknow hello BGP partnerský vztah hello jako číslo, které vaše místní síť VPN a IP adresy zařízení používá.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-183">If your local VPN device supports hello BGP and you want tooenable dynamic routing, you need tooknow hello BGP peering IP and hello AS number that your local VPN device uses.</span></span>

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for hello BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. <span data-ttu-id="ceb1f-184">Nakonfigurujte místní zařízení tooconnect toohello nové Azure VPN bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-184">Configure your local VPN device tooconnect toohello new Azure VPN gateway.</span></span> <span data-ttu-id="ceb1f-185">Další informace o konfiguraci zařízení VPN najdete v tématu [Konfigurace zařízení VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-185">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
9. <span data-ttu-id="ceb1f-186">Brána Site-to-Site VPN hello odkaz na Azure toohello místní brány.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-186">Link hello Site-to-Site VPN gateway on Azure toohello local gateway.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <span data-ttu-id="ceb1f-187"><a name="add"></a>tooconfigure koexistujících připojení pro už existující virtuální síť</span><span class="sxs-lookup"><span data-stu-id="ceb1f-187"><a name="add"></a>tooconfigure coexisting connections for an already existing VNet</span></span>
<span data-ttu-id="ceb1f-188">Pokud máte existující virtuální síť, zkontrolujte velikost podsítě brány hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-188">If you have an existing virtual network, check hello gateway subnet size.</span></span> <span data-ttu-id="ceb1f-189">Pokud podsíť brány hello velikosti/28 nebo/29, musíte nejprve odstranit bránu virtuální sítě hello a zvýšit velikost podsítě brány hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-189">If hello gateway subnet is /28 or /29, you must first delete hello virtual network gateway and increase hello gateway subnet size.</span></span> <span data-ttu-id="ceb1f-190">Hello kroky v této části ukazují, jak toodo který.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-190">hello steps in this section show you how toodo that.</span></span>

<span data-ttu-id="ceb1f-191">Pokud hello podsíť brány je/27 nebo větší a hello virtuální síť je připojená přes ExpressRoute, můžete přeskočit hello kroky a přejít příliš["Krok 6 – Vytvoření brány Site-to-Site VPN"](#vpngw) v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-191">If hello gateway subnet is /27 or larger and hello virtual network is connected via ExpressRoute, you can skip hello steps below and proceed too["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in hello previous section.</span></span> 

> [!NOTE]
> <span data-ttu-id="ceb1f-192">Pokud odstraníte existující bránu hello, místní místo ztratí hello připojení tooyour virtuální sítě během práce na této konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-192">When you delete hello existing gateway, your local premises will lose hello connection tooyour virtual network while you are working on this configuration.</span></span> 
> 
> 

1. <span data-ttu-id="ceb1f-193">Budete potřebovat tooinstall hello nejnovější verzi rutin prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-193">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="ceb1f-194">Další informace o instalaci rutin najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-194">For more information about installing cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="ceb1f-195">Hello rutin, které používáte pro tuto konfiguraci můžou mírně lišit, než co je znají.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-195">hello cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="ceb1f-196">Být jisti toouse hello rutiny určené v těchto pokynech.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-196">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="ceb1f-197">Odstraňte existující bránu ExpressRoute nebo VPN typu Site-to-Site na hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-197">Delete hello existing ExpressRoute or Site-to-Site VPN gateway.</span></span>

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. <span data-ttu-id="ceb1f-198">Odstraňte podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-198">Delete Gateway Subnet.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="ceb1f-199">Přidejte podsíť brány, která je /27 nebo větší.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-199">Add a Gateway Subnet that is /27 or larger.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ceb1f-200">Pokud nemáte dost IP adres v velikost podsítě brány vaší virtuální sítě tooincrease hello, musíte tooadd další adresní prostor IP adres.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-200">If you don't have enough IP addresses left in your virtual network tooincrease hello gateway subnet size, you need tooadd more IP address space.</span></span>
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="ceb1f-201">Uložte konfiguraci virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-201">Save hello VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. <span data-ttu-id="ceb1f-202">V tuto chvíli máte virtuální síť, která nemá žádné brány.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-202">At this point, you have a VNet with no gateways.</span></span> <span data-ttu-id="ceb1f-203">toocreate nové brány a dokončili připojení, abyste mohli pokračovat [krokem 4 – vytvoření brány ExpressRoute](#gw), který se nachází v hello předcházející sadě kroků.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-203">toocreate new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in hello preceding set of steps.</span></span>

## <a name="tooadd-point-to-site-configuration-toohello-vpn-gateway"></a><span data-ttu-id="ceb1f-204">Brána sítě VPN toohello tooadd konfigurace point-to-site</span><span class="sxs-lookup"><span data-stu-id="ceb1f-204">tooadd point-to-site configuration toohello VPN gateway</span></span>
<span data-ttu-id="ceb1f-205">Můžete provést kroky hello níže tooadd Point-to-Site konfigurace tooyour brány VPN v nastavení koexistence.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-205">You can follow hello steps below tooadd Point-to-Site configuration tooyour VPN gateway in a co-existence setup.</span></span>

1. <span data-ttu-id="ceb1f-206">Přidejte fond adres klienta VPN.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-206">Add VPN Client address pool.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. <span data-ttu-id="ceb1f-207">Nahrajte hello VPN kořenový certifikát tooAzure pro bránu sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-207">Upload hello VPN root certificate tooAzure for your VPN gateway.</span></span> <span data-ttu-id="ceb1f-208">V tomto příkladu se předpokládá, že tento hello kořenový certifikát je uložený v hello místního počítače, kde se spouštějí hello následující rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ceb1f-208">In this example, it's assumed that hello root certificate is stored in hello local machine where hello following PowerShell cmdlets are run.</span></span>

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

<span data-ttu-id="ceb1f-209">Další informace o VPN typu point-to-site najdete v tématu [Konfigurace připojení typu point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-209">For more information on Point-to-Site VPN, see [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ceb1f-210">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ceb1f-210">Next steps</span></span>
<span data-ttu-id="ceb1f-211">Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="ceb1f-211">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
