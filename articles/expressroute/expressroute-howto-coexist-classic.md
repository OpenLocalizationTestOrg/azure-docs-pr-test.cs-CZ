---
title: "Konfigurace ExpressRoute a připojení VPN typu site-to-site, která mohou existovat vedle sebe: Classic: Azure | Dokumentace Microsoftu"
description: "Tento článek vás provede konfigurací ExpressRoute a připojení VPN typu site-to-site, která mohou v modelu nasazení Classic existovat vedle sebe."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: dcf1a5af-a289-466a-b812-0bfedbd2bda0
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: 09d1649f0ca0cf4ca464d95b29461cad3fe51788
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a><span data-ttu-id="69a7f-103">Konfigurace společně používaných připojení typu Site-to-Site a ExpressRoute (Classic)</span><span class="sxs-lookup"><span data-stu-id="69a7f-103">Configure ExpressRoute and Site-to-Site coexisting connections (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="69a7f-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="69a7f-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="69a7f-105">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="69a7f-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="69a7f-106">Možnost konfigurace VPN typu site-to-site a ExpressRoute má několik výhod.</span><span class="sxs-lookup"><span data-stu-id="69a7f-106">Having the ability to configure Site-to-Site VPN and ExpressRoute has several advantages.</span></span> <span data-ttu-id="69a7f-107">Můžete nakonfigurovat VPN typu site-to-site jako zabezpečenou cestu převzetí služeb při selhání pro ExressRoute, nebo použít VPN typu site-to-site pro připojení k webům, které nejsou připojené prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-107">You can configure Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs to connect to sites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="69a7f-108">V tomto článku vám nabídneme postupy konfigurace pro oba scénáře.</span><span class="sxs-lookup"><span data-stu-id="69a7f-108">We will cover the steps to configure both scenarios in this article.</span></span> <span data-ttu-id="69a7f-109">Tento článek se týká modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="69a7f-109">This article applies to the classic deployment model.</span></span> <span data-ttu-id="69a7f-110">Tato konfigurace není k dispozici na portálu.</span><span class="sxs-lookup"><span data-stu-id="69a7f-110">This configuration is not available in the portal.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="69a7f-111">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="69a7f-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="69a7f-112">Než budete postupovat podle dál uvedených pokynů, musí být předem nakonfigurované okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-112">ExpressRoute circuits must be pre-configured before you follow the instructions below.</span></span> <span data-ttu-id="69a7f-113">Před provedením následujících kroků zkontrolujte, že jste provedli postupy pro [vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a [konfiguraci směrování](expressroute-howto-routing-classic.md).</span><span class="sxs-lookup"><span data-stu-id="69a7f-113">Make sure that you have followed the guides to [create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and [configure routing](expressroute-howto-routing-classic.md) before you follow the steps below.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="69a7f-114">Omezení</span><span class="sxs-lookup"><span data-stu-id="69a7f-114">Limits and limitations</span></span>
* <span data-ttu-id="69a7f-115">**Směrování provozu není podporováno.**</span><span class="sxs-lookup"><span data-stu-id="69a7f-115">**Transit routing is not supported.**</span></span> <span data-ttu-id="69a7f-116">Nemůžete provádět směrování (přes Azure) mezi místní sítí připojenou prostřednictvím sítě VPN typu site-to-site a místní sítí připojenou přes ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-116">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="69a7f-117">**Typ point-to-site není podporován.**</span><span class="sxs-lookup"><span data-stu-id="69a7f-117">**Point-to-site is not supported.**</span></span> <span data-ttu-id="69a7f-118">Nemůžete povolit připojení VPN typu point-to-site ke stejné virtuální síti, která je připojená k ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-118">You can't enable point-to-site VPN connections to the same VNet that is connected to ExpressRoute.</span></span> <span data-ttu-id="69a7f-119">Připojení VPN typu point-to-site a ExpressRoute nemůžou existovat pro stejnou síť VNet souběžně.</span><span class="sxs-lookup"><span data-stu-id="69a7f-119">Point-to-site VPN and ExpressRoute cannot coexist for the same VNet.</span></span>
* <span data-ttu-id="69a7f-120">**Na bráně VPN typu site-to-site nelze povolit vynucené tunelové propojení.**</span><span class="sxs-lookup"><span data-stu-id="69a7f-120">**Forced tunneling cannot be enabled on the Site-to-Site VPN gateway.**</span></span> <span data-ttu-id="69a7f-121">Můžete pouze „vynutit“ veškerý provoz směřující na internet zpět do místní sítě prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-121">You can only "force" all Internet-bound traffic back to your on-premises network via ExpressRoute.</span></span>
* <span data-ttu-id="69a7f-122">**Základní brána SKU není podporována.**</span><span class="sxs-lookup"><span data-stu-id="69a7f-122">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="69a7f-123">Pro [bránu ExpressRoute](expressroute-about-virtual-network-gateways.md) a [bránu VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) je nutné použít jinou než základní bránu SKU.</span><span class="sxs-lookup"><span data-stu-id="69a7f-123">You must use a non-Basic SKU gateway for both the [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and the [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="69a7f-124">**Podporována je pouze brána VPN na základě tras.**</span><span class="sxs-lookup"><span data-stu-id="69a7f-124">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="69a7f-125">Je nutné použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) na základě tras.</span><span class="sxs-lookup"><span data-stu-id="69a7f-125">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="69a7f-126">**Pro vaši bránu VPN by měla být nakonfigurována statická trasa.**</span><span class="sxs-lookup"><span data-stu-id="69a7f-126">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="69a7f-127">Pokud je vaše místní síť připojená k ExpressRoute a síti VPN typu site-to-site, musíte mít v místní síti konfigurovanou statickou trasu, abyste mohli směrovat připojení VPN typu site-to-site do veřejného internetu.</span><span class="sxs-lookup"><span data-stu-id="69a7f-127">If your local network is connected to both ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network to route the Site-to-Site VPN connection to the public Internet.</span></span>
* <span data-ttu-id="69a7f-128">**Nejprve je nutné nakonfigurovat bránu ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="69a7f-128">**ExpressRoute gateway must be configured first.**</span></span> <span data-ttu-id="69a7f-129">Bránu ExpressRoute musíte vytvořit předtím, než přidáte bránu VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="69a7f-129">You must create the ExpressRoute gateway first before you add the Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="69a7f-130">Návrhy konfigurace</span><span class="sxs-lookup"><span data-stu-id="69a7f-130">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="69a7f-131">Konfigurace VPN typu site-to-site jako cesty převzetí služeb při selhání pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="69a7f-131">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="69a7f-132">Můžete nakonfigurovat připojení VPN typu site-to-site jako záložní pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-132">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="69a7f-133">To platí jenom pro virtuální sítě, které jsou propojené s cestou soukromého partnerského vztahu Azure.</span><span class="sxs-lookup"><span data-stu-id="69a7f-133">This applies only to virtual networks linked to the Azure private peering path.</span></span> <span data-ttu-id="69a7f-134">Neexistuje žádné řešení převzetí služeb při selhání založené na VPN pro služby, které jsou přístupné prostřednictvím veřejného partnerského vztahu Azure nebo partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="69a7f-134">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="69a7f-135">Okruh ExpressRoute je vždy primárním propojením.</span><span class="sxs-lookup"><span data-stu-id="69a7f-135">The ExpressRoute circuit is always the primary link.</span></span> <span data-ttu-id="69a7f-136">Data budou procházet cestou VPN typu site-to-site jenom v případě, když okruh ExpressRoute selže.</span><span class="sxs-lookup"><span data-stu-id="69a7f-136">Data will flow through the Site-to-Site VPN path only if the ExpressRoute circuit fails.</span></span> 

> [!NOTE]
> <span data-ttu-id="69a7f-137">I když v případě, že jsou obě trasy stejné, je okruh ExpressRoute upřednostněný před VPN typu Site-to-Site, Azure k výběru trasy směrem k cíli paketu použije nejdelší shodu předpony.</span><span class="sxs-lookup"><span data-stu-id="69a7f-137">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are the same, Azure will use the longest prefix match to choose the route towards the packet's destination.</span></span>
> 
> 

![Současná existence](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a><span data-ttu-id="69a7f-139">Konfigurace VPN typu site-to-site pro připojení webů, které nejsou připojené prostřednictvím ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="69a7f-139">Configure a Site-to-Site VPN to connect to sites not connected through ExpressRoute</span></span>
<span data-ttu-id="69a7f-140">Svoji síť můžete nakonfigurovat tak, že některé weby jsou připojené přímo k Azure prostřednictvím VPN typu site-to-site a některé weby přes ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-140">You can configure your network where some sites connect directly to Azure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Současná existence](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="69a7f-142">Virtuální síť nemůžete nakonfigurovat jako směrovač provozu.</span><span class="sxs-lookup"><span data-stu-id="69a7f-142">You cannot a configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-the-steps-to-use"></a><span data-ttu-id="69a7f-143">Výběr kroků k použití</span><span class="sxs-lookup"><span data-stu-id="69a7f-143">Selecting the steps to use</span></span>
<span data-ttu-id="69a7f-144">Existují dvě sady postupů, ze kterých si můžete vybrat, když konfigurujete připojení, která můžou existovat společně.</span><span class="sxs-lookup"><span data-stu-id="69a7f-144">There are two different sets of procedures to choose from in order to configure connections that can coexist.</span></span> <span data-ttu-id="69a7f-145">Postup konfigurace, který vyberete, bude záviset na tom, jestli máte existující virtuální síť, ke které se chcete připojit, nebo chcete vytvořit novou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="69a7f-145">The configuration procedure that you select will depend on whether you have an existing virtual network that you want to connect to, or you want to create a new virtual network.</span></span>

* <span data-ttu-id="69a7f-146">Nemám virtuální síť a potřebuji ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="69a7f-146">I don't have a VNet and need to create one.</span></span>
  
    <span data-ttu-id="69a7f-147">Pokud ještě nemáte virtuální síť, tento postup vás provede procesem vytvoření nové virtuální sítě pomocí modelu nasazení Classic a vytvoření nových připojení ExpressRoute a VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="69a7f-147">If you don’t already have a virtual network, this procedure will walk you through creating a new virtual network using the classic deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="69a7f-148">Konfiguraci provedete podle kroků v části [Vytvoření nové virtuální sítě a koexistujících připojení](#new).</span><span class="sxs-lookup"><span data-stu-id="69a7f-148">To configure, follow the steps in the article section [To create a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="69a7f-149">Už mám virtuální síť modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="69a7f-149">I already have a classic deployment model VNet.</span></span>
  
    <span data-ttu-id="69a7f-150">Už můžete mít virtuální síť s existujícím připojením VPN typu site-to-site nebo připojením ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-150">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="69a7f-151">V části [Konfigurace koexistujících připojení pro už existující virtuální síť](#add) najdete postup odstranění brány a následného vytvoření nových připojení ExpressRoute a VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="69a7f-151">The article section [To configure coexsiting connections for an already existing VNet](#add) will walk you through deleting the gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="69a7f-152">Uvědomte si, že při vytváření nových připojení musí být kroky provedené ve velmi specifickém pořadí.</span><span class="sxs-lookup"><span data-stu-id="69a7f-152">Note that when creating the new connections, the steps must be completed in a very specific order.</span></span> <span data-ttu-id="69a7f-153">Nepoužívejte pro vytvoření připojení a bran pokyny z jiných článků.</span><span class="sxs-lookup"><span data-stu-id="69a7f-153">Don't use the instructions in other articles to create your gateways and connections.</span></span>
  
    <span data-ttu-id="69a7f-154">V tomto postupu bude vytvoření připojení, která mohou existovat společně, vyžadovat, abyste odstranili bránu a pak nakonfigurovali nové brány.</span><span class="sxs-lookup"><span data-stu-id="69a7f-154">In this procedure, creating connections that can coexist will require you to delete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="69a7f-155">To znamená, že budete mít během odstraňování a opětného vytváření brány a připojení výpadek připojení mezi místy, ale nebude nutné migrovat žádné virtuální počítače a služby do nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="69a7f-155">This means you will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need to migrate any of your VMs or services to a new virtual network.</span></span> <span data-ttu-id="69a7f-156">Virtuální počítače a služby budou během konfigurace brány stále schopné komunikovat prostřednictvím nástroje pro vyrovnávání zatížení, pokud jsou tak nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="69a7f-156">Your VMs and services will still be able to communicate out through the load balancer while you configure your gateway if they are configured to do so.</span></span>

## <span data-ttu-id="69a7f-157"><a name="new"></a>Vytvoření nové virtuální sítě a současně existujících připojení</span><span class="sxs-lookup"><span data-stu-id="69a7f-157"><a name="new"></a>To create a new virtual network and coexisting connections</span></span>
<span data-ttu-id="69a7f-158">Tento postup vás provede procesem vytvoření virtuální sítě a vytvoření připojení ExpressRoute a VPN site-to-site, která budou existovat společně.</span><span class="sxs-lookup"><span data-stu-id="69a7f-158">This procedure will walk you through creating a VNet and create Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="69a7f-159">Budete potřebovat nainstalovat nejnovější verzi rutin Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="69a7f-159">You'll need to install the latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="69a7f-160">Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="69a7f-160">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span> <span data-ttu-id="69a7f-161">Všimněte si, že rutiny, které budete používat pro tuto konfiguraci, se můžou mírně lišit od těch, co znáte.</span><span class="sxs-lookup"><span data-stu-id="69a7f-161">Note that the cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="69a7f-162">Ujistěte se, že používáte rutiny určené v těchto pokynech.</span><span class="sxs-lookup"><span data-stu-id="69a7f-162">Be sure to use the cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="69a7f-163">Vytvořte schéma pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="69a7f-163">Create a schema for your virtual network.</span></span> <span data-ttu-id="69a7f-164">Další informace o schématu konfigurace najdete v tématu [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx) (Schéma konfigurace Azure Virtual Network).</span><span class="sxs-lookup"><span data-stu-id="69a7f-164">For more information about the configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   
    <span data-ttu-id="69a7f-165">Při vytváření schématu použijte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="69a7f-165">When you create your schema, make sure you use the following values:</span></span>
   
   * <span data-ttu-id="69a7f-166">Podsíť brány pro virtuální síť musí být /27 nebo kratší předpona (například /26 nebo /25).</span><span class="sxs-lookup"><span data-stu-id="69a7f-166">The gateway subnet for the virtual network must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   * <span data-ttu-id="69a7f-167">Typ připojení brány je Vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="69a7f-167">The gateway connection type is "Dedicated".</span></span>
     
             <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
               <AddressSpace>
                 <AddressPrefix>10.17.159.192/26</AddressPrefix>
               </AddressSpace>
               <Subnets>
                 <Subnet name="Subnet-1">
                   <AddressPrefix>10.17.159.192/27</AddressPrefix>
                 </Subnet>
                 <Subnet name="GatewaySubnet">
                   <AddressPrefix>10.17.159.224/27</AddressPrefix>
                 </Subnet>
               </Subnets>
               <Gateway>
                 <ConnectionsToLocalNetwork>
                   <LocalNetworkSiteRef name="MyLocalNetwork">
                     <Connection type="Dedicated" />
                   </LocalNetworkSiteRef>
                 </ConnectionsToLocalNetwork>
               </Gateway>
             </VirtualNetworkSite>
3. <span data-ttu-id="69a7f-168">Po vytvoření a konfiguraci souboru schématu XML tento soubor odešlete.</span><span class="sxs-lookup"><span data-stu-id="69a7f-168">After creating and configuring your xml schema file, upload the file.</span></span> <span data-ttu-id="69a7f-169">Tím vytvoříte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="69a7f-169">This will create your virtual network.</span></span>
   
    <span data-ttu-id="69a7f-170">Použijte následující rutinu k odeslání souboru (po náhradě vlastní hodnotou).</span><span class="sxs-lookup"><span data-stu-id="69a7f-170">Use the following cmdlet to upload your file, replacing the value with your own.</span></span>
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <span data-ttu-id="69a7f-171"><a name="gw"></a>Vytvořte bránu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-171"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="69a7f-172">Je nutné zadat GatewaySKU jako *Standard*, *HighPerformance* nebo *UltraPerformance* a GatewayType jako *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="69a7f-172">Be sure to specify the GatewaySKU as *Standard*, *HighPerformance*, or *UltraPerformance* and the GatewayType as *DynamicRouting*.</span></span>
   
    <span data-ttu-id="69a7f-173">Použijte následující příklad a nahraďte v něm hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="69a7f-173">Use the following sample, substituting the values for your own.</span></span>
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. <span data-ttu-id="69a7f-174">Propojte bránu ExpressRoute s okruhem ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="69a7f-174">Link the ExpressRoute gateway to the ExpressRoute circuit.</span></span> <span data-ttu-id="69a7f-175">Po dokončení tohoto kroku bude připojení mezi místní sítí a Azure prostřednictvím ExpressRoute vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="69a7f-175">After this step has been completed, the connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span>
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <span data-ttu-id="69a7f-176"><a name="vpngw"></a>Dále vytvořte bránu VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="69a7f-176"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="69a7f-177">GatewaySKU musí být *Standard*, *HighPerformance* nebo *UltraPerformance* a GatewayType musí být *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="69a7f-177">The GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance* and the GatewayType must be *DynamicRouting*.</span></span>
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    <span data-ttu-id="69a7f-178">Pokud chcete načíst nastavení brány virtuální sítě, včetně ID brány a veřejné IP adresy, použijte rutinu `Get-AzureVirtualNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="69a7f-178">To retrieve the virtual network gateway settings, including the gateway ID and the public IP, use the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span>
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for the following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded
7. <span data-ttu-id="69a7f-179">Vytvořte entitu brány VPN místního webu.</span><span class="sxs-lookup"><span data-stu-id="69a7f-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="69a7f-180">Tento příkaz neprovede konfiguraci vaší místní brány VPN.</span><span class="sxs-lookup"><span data-stu-id="69a7f-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="69a7f-181">Místo toho umožní zadat nastavení místní brány, jako je například veřejná IP adresa a místní adresní prostor, aby se brána Azure VPN k nim mohla připojit.</span><span class="sxs-lookup"><span data-stu-id="69a7f-181">Rather, it allows you to provide the local gateway settings, such as the public IP and the on-premises address space, so that the Azure VPN gateway can connect to it.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="69a7f-182">Místní web připojení VPN typu site-to-site není v souboru netcfg definován.</span><span class="sxs-lookup"><span data-stu-id="69a7f-182">The local site for the Site-to-Site VPN is not defined in the netcfg.</span></span> <span data-ttu-id="69a7f-183">Místo toho musíte k určení parametrů místního webu použít tuto rutinu.</span><span class="sxs-lookup"><span data-stu-id="69a7f-183">Instead, you must use this cmdlet to specify the local site parameters.</span></span> <span data-ttu-id="69a7f-184">Nelze je definovat pomocí portálu nebo souboru netcfg.</span><span class="sxs-lookup"><span data-stu-id="69a7f-184">You cannot define it using either portal, or the netcfg file.</span></span>
   > 
   > 
   
    <span data-ttu-id="69a7f-185">Použijte následující příklad a nahraďte v něm hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="69a7f-185">Use the following sample, replacing the values with your own.</span></span>
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > <span data-ttu-id="69a7f-186">Pokud vaše místní síť obsahuje víc tras, můžete je předat všechny najednou jako pole.</span><span class="sxs-lookup"><span data-stu-id="69a7f-186">If your local network has multiple routes, you can pass them all in as an array.</span></span>  <span data-ttu-id="69a7f-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span><span class="sxs-lookup"><span data-stu-id="69a7f-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span></span>  
   > 
   > 

    <span data-ttu-id="69a7f-188">Pokud chcete načíst nastavení brány virtuální sítě, včetně ID brány a veřejné IP adresy, použijte rutinu `Get-AzureVirtualNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="69a7f-188">To retrieve the virtual network gateway settings, including the gateway ID and the public IP, use the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="69a7f-189">Prohlédněte si následující příklad.</span><span class="sxs-lookup"><span data-stu-id="69a7f-189">See the following example.</span></span>

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. <span data-ttu-id="69a7f-190">Nakonfigurujte místní zařízení VPN pro připojení k nové bráně.</span><span class="sxs-lookup"><span data-stu-id="69a7f-190">Configure your local VPN device to connect to the new gateway.</span></span> <span data-ttu-id="69a7f-191">Při konfiguraci zařízení VPN použijte informace, které jste získali v kroku 6.</span><span class="sxs-lookup"><span data-stu-id="69a7f-191">Use the information that you retrieved in step 6 when configuring your VPN device.</span></span> <span data-ttu-id="69a7f-192">Další informace o konfiguraci zařízení VPN najdete v tématu [Konfigurace zařízení VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="69a7f-192">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
2. <span data-ttu-id="69a7f-193">Propojte bránu VPN typu site-to-site v Azure s místní bránou.</span><span class="sxs-lookup"><span data-stu-id="69a7f-193">Link the Site-to-Site VPN gateway on Azure to the local gateway.</span></span>
   
    <span data-ttu-id="69a7f-194">V tomto příkladu je ID místní brány connectedEntityId, které můžete najít spuštěním rutiny `Get-AzureLocalNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="69a7f-194">In this example, connectedEntityId is the local gateway ID, which you can find by running `Get-AzureLocalNetworkGateway`.</span></span> <span data-ttu-id="69a7f-195">VirtualNetworkGatewayId můžete najít pomocí rutiny `Get-AzureVirtualNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="69a7f-195">You can find virtualNetworkGatewayId by using the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="69a7f-196">Po dokončení tohoto kroku bude připojení mezi místní sítí a Azure prostřednictvím připojení VPN typu site-to-site vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="69a7f-196">After this step, the connection between your local network and Azure via the Site-to-Site VPN connection is established.</span></span>

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <span data-ttu-id="69a7f-197"><a name="add"></a>Konfigurace současně existujících připojení pro už existující virtuální síť</span><span class="sxs-lookup"><span data-stu-id="69a7f-197"><a name="add"></a>To configure coexsiting connections for an already existing VNet</span></span>
<span data-ttu-id="69a7f-198">Pokud máte existující virtuální síť, zkontrolujte velikost podsítě brány.</span><span class="sxs-lookup"><span data-stu-id="69a7f-198">If you have an existing virtual network, check the gateway subnet size.</span></span> <span data-ttu-id="69a7f-199">Pokud podsíť brány je /28 nebo /29, musíte nejdřív bránu virtuální sítě odstranit a zvýšit velikost podsítě brány.</span><span class="sxs-lookup"><span data-stu-id="69a7f-199">If the gateway subnet is /28 or /29, you must first delete the virtual network gateway and increase the gateway subnet size.</span></span> <span data-ttu-id="69a7f-200">Postup v této části ukazuje, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="69a7f-200">The steps in this section will show you how to do that.</span></span>

<span data-ttu-id="69a7f-201">Pokud podsíť brány je /27 nebo větší a virtuální síť je připojená přes ExpressRoute, můžete přeskočit následující kroky a přejít ke [kroku 6 – Vytvoření brány VPN typu site-to-site](#vpngw) v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="69a7f-201">If the gateway subnet is /27 or larger and the virtual network is connected via ExpressRoute, you can skip the steps below and proceed to ["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in the previous section.</span></span>

> [!NOTE]
> <span data-ttu-id="69a7f-202">Pokud odstraníte existující bránu, místní místo ztratí během práce na této konfiguraci připojení k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="69a7f-202">When you delete the existing gateway, your local premises will lose the connection to your virtual network while you are working on this configuration.</span></span>
> 
> 

1. <span data-ttu-id="69a7f-203">Budete potřebovat nainstalovat nejnovější verzi rutin PowerShellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="69a7f-203">You'll need to install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="69a7f-204">Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="69a7f-204">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span> <span data-ttu-id="69a7f-205">Všimněte si, že rutiny, které budete používat pro tuto konfiguraci, se můžou mírně lišit od těch, co znáte.</span><span class="sxs-lookup"><span data-stu-id="69a7f-205">Note that the cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="69a7f-206">Ujistěte se, že používáte rutiny určené v těchto pokynech.</span><span class="sxs-lookup"><span data-stu-id="69a7f-206">Be sure to use the cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="69a7f-207">Odstraňte existující bránu ExpressRoute nebo VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="69a7f-207">Delete the existing ExpressRoute or Site-to-Site VPN gateway.</span></span> <span data-ttu-id="69a7f-208">Použijte následující rutinu a nahraďte v ní hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="69a7f-208">Use the following cmdlet, replacing the values with your own.</span></span>
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. <span data-ttu-id="69a7f-209">Exportujte schéma virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="69a7f-209">Export the virtual network schema.</span></span> <span data-ttu-id="69a7f-210">Použijte následující rutinu PowerShellu a nahraďte v ní hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="69a7f-210">Use the following PowerShell cmdlet, replacing the values with your own.</span></span>
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. <span data-ttu-id="69a7f-211">Upravte schéma konfiguračního souboru sítě, aby podsíť brány byla /27 nebo kratší předpona (například /26 nebo /25).</span><span class="sxs-lookup"><span data-stu-id="69a7f-211">Edit the network configuration file schema so that the gateway subnet is /27 or a shorter prefix (such as /26 or /25).</span></span> <span data-ttu-id="69a7f-212">Prohlédněte si následující příklad.</span><span class="sxs-lookup"><span data-stu-id="69a7f-212">See the following example.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="69a7f-213">Pokud vám ve virtuální síti nezbylo dost IP adres pro zvětšení velikosti podsítě brány, budete muset přidat další adresní prostor IP adres.</span><span class="sxs-lookup"><span data-stu-id="69a7f-213">If you don't have enough IP addresses left in your virtual network to increase the gateway subnet size, you need to add more IP address space.</span></span> <span data-ttu-id="69a7f-214">Další informace o schématu konfigurace najdete v tématu [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx) (Schéma konfigurace Azure Virtual Network).</span><span class="sxs-lookup"><span data-stu-id="69a7f-214">For more information about the configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. <span data-ttu-id="69a7f-215">Pokud předchozí brána byla VPN typu site-to-site, musíte změnit taky typ připojení na **Dedicated**.</span><span class="sxs-lookup"><span data-stu-id="69a7f-215">If your previous gateway was a Site-to-Site VPN, you must also change the connection type to **Dedicated**.</span></span>
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. <span data-ttu-id="69a7f-216">V tuto chvíli máte virtuální síť, která nemá žádné brány.</span><span class="sxs-lookup"><span data-stu-id="69a7f-216">At this point, you'll have a VNet with no gateways.</span></span> <span data-ttu-id="69a7f-217">Abyste vytvořili nové brány a dokončili připojení, můžete pokračovat [krokem 4 – Vytvoření brány ExpressRoute](#gw), který se nachází v předchozí sadě kroků.</span><span class="sxs-lookup"><span data-stu-id="69a7f-217">To create new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in the preceding set of steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69a7f-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69a7f-218">Next steps</span></span>
<span data-ttu-id="69a7f-219">Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="69a7f-219">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md)</span></span>

