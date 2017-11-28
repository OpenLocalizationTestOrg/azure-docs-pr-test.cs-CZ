---
title: "Konfigurace ExpressRoute a připojení VPN typu site-to-site, která mohou existovat vedle sebe: Classic: Azure | Dokumentace Microsoftu"
description: "Tento článek vás provede konfigurací ExpressRoute a připojení Site-to-Site VPN, která mohou existovat vedle sebe pro model nasazení classic hello."
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
ms.openlocfilehash: abb30fff55e8ec243f2920c5b2f70c43717755fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a><span data-ttu-id="b270c-103">Konfigurace společně používaných připojení typu Site-to-Site a ExpressRoute (Classic)</span><span class="sxs-lookup"><span data-stu-id="b270c-103">Configure ExpressRoute and Site-to-Site coexisting connections (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b270c-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b270c-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="b270c-105">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="b270c-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="b270c-106">Hello možnost tooconfigure VPN Site-to-Site a ExpressRoute má několik výhod.</span><span class="sxs-lookup"><span data-stu-id="b270c-106">Having hello ability tooconfigure Site-to-Site VPN and ExpressRoute has several advantages.</span></span> <span data-ttu-id="b270c-107">Můžete nakonfigurovat VPN typu Site-to-Site jako cestu zabezpečené převzetí služeb při selhání pro ExressRoute, nebo použít toosites tooconnect sítě Site-to-Site VPN, které nejsou připojené prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b270c-107">You can configure Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs tooconnect toosites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="b270c-108">Vám nabídneme hello kroky tooconfigure oba scénáře v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="b270c-108">We will cover hello steps tooconfigure both scenarios in this article.</span></span> <span data-ttu-id="b270c-109">Tento článek se týká modelu nasazení classic toohello.</span><span class="sxs-lookup"><span data-stu-id="b270c-109">This article applies toohello classic deployment model.</span></span> <span data-ttu-id="b270c-110">Tato konfigurace není v portálu hello k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b270c-110">This configuration is not available in hello portal.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="b270c-111">**O modelech nasazení Azure**</span><span class="sxs-lookup"><span data-stu-id="b270c-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="b270c-112">Okruhy ExpressRoute musí být předem nakonfigurované, než začnete postupovat podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-112">ExpressRoute circuits must be pre-configured before you follow hello instructions below.</span></span> <span data-ttu-id="b270c-113">Ujistěte se, že jste postupovali podle příručky hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md) a [konfigurace směrování](expressroute-howto-routing-classic.md) před provedením následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-113">Make sure that you have followed hello guides too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and [configure routing](expressroute-howto-routing-classic.md) before you follow hello steps below.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="b270c-114">Omezení</span><span class="sxs-lookup"><span data-stu-id="b270c-114">Limits and limitations</span></span>
* <span data-ttu-id="b270c-115">**Směrování provozu není podporováno.**</span><span class="sxs-lookup"><span data-stu-id="b270c-115">**Transit routing is not supported.**</span></span> <span data-ttu-id="b270c-116">Nemůžete provádět směrování (přes Azure) mezi místní sítí připojenou prostřednictvím sítě VPN typu site-to-site a místní sítí připojenou přes ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b270c-116">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="b270c-117">**Typ point-to-site není podporován.**</span><span class="sxs-lookup"><span data-stu-id="b270c-117">**Point-to-site is not supported.**</span></span> <span data-ttu-id="b270c-118">Nelze povolit toohello připojení point-to-site VPN stejnou virtuální síť, která je připojená tooExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b270c-118">You can't enable point-to-site VPN connections toohello same VNet that is connected tooExpressRoute.</span></span> <span data-ttu-id="b270c-119">Síť VPN Point-to-site a ExpressRoute nemůžou existovat pro hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="b270c-119">Point-to-site VPN and ExpressRoute cannot coexist for hello same VNet.</span></span>
* <span data-ttu-id="b270c-120">**Na hello brány VPN Site-to-Site nejde povolit vynucené tunelování.**</span><span class="sxs-lookup"><span data-stu-id="b270c-120">**Forced tunneling cannot be enabled on hello Site-to-Site VPN gateway.**</span></span> <span data-ttu-id="b270c-121">Můžete jenom "Vynutit" všechny internetový provoz back tooyour místní sítě prostřednictvím ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b270c-121">You can only "force" all Internet-bound traffic back tooyour on-premises network via ExpressRoute.</span></span>
* <span data-ttu-id="b270c-122">**Základní brána SKU není podporována.**</span><span class="sxs-lookup"><span data-stu-id="b270c-122">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="b270c-123">Musíte použít bránu bez – základní SKU pro obě hello [brány ExpressRoute](expressroute-about-virtual-network-gateways.md) a hello [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="b270c-123">You must use a non-Basic SKU gateway for both hello [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and hello [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="b270c-124">**Podporována je pouze brána VPN na základě tras.**</span><span class="sxs-lookup"><span data-stu-id="b270c-124">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="b270c-125">Je nutné použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) na základě tras.</span><span class="sxs-lookup"><span data-stu-id="b270c-125">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="b270c-126">**Pro vaši bránu VPN by měla být nakonfigurována statická trasa.**</span><span class="sxs-lookup"><span data-stu-id="b270c-126">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="b270c-127">Pokud vaše místní síť připojená tooboth ExpressRoute a Site-to-Site VPN, musíte mít ve vaší místní síti tooroute hello Site-to-Site VPN připojení toohello konfigurovanou statickou trasu veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="b270c-127">If your local network is connected tooboth ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network tooroute hello Site-to-Site VPN connection toohello public Internet.</span></span>
* <span data-ttu-id="b270c-128">**Nejprve je nutné nakonfigurovat bránu ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="b270c-128">**ExpressRoute gateway must be configured first.**</span></span> <span data-ttu-id="b270c-129">Předtím, než přidáte bránu VPN typu Site-to-Site hello musí nejprve vytvořte bránu ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-129">You must create hello ExpressRoute gateway first before you add hello Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="b270c-130">Návrhy konfigurace</span><span class="sxs-lookup"><span data-stu-id="b270c-130">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="b270c-131">Konfigurace VPN typu site-to-site jako cesty převzetí služeb při selhání pro ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="b270c-131">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="b270c-132">Můžete nakonfigurovat připojení VPN typu site-to-site jako záložní pro ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b270c-132">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="b270c-133">To platí pouze toovirtual sítě propojené toohello cestou soukromého partnerského vztahu Azure.</span><span class="sxs-lookup"><span data-stu-id="b270c-133">This applies only toovirtual networks linked toohello Azure private peering path.</span></span> <span data-ttu-id="b270c-134">Neexistuje žádné řešení převzetí služeb při selhání založené na VPN pro služby, které jsou přístupné prostřednictvím veřejného partnerského vztahu Azure nebo partnerského vztahu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="b270c-134">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="b270c-135">Hello okruh ExpressRoute je vždy hello primární propojení.</span><span class="sxs-lookup"><span data-stu-id="b270c-135">hello ExpressRoute circuit is always hello primary link.</span></span> <span data-ttu-id="b270c-136">Data budou procházet prostřednictvím cesty hello Site-to-Site VPN, pouze pokud hello okruh ExpressRoute selže.</span><span class="sxs-lookup"><span data-stu-id="b270c-136">Data will flow through hello Site-to-Site VPN path only if hello ExpressRoute circuit fails.</span></span> 

> [!NOTE]
> <span data-ttu-id="b270c-137">Při okruh ExpressRoute je upřednostňované prostřednictvím sítě Site-to-Site VPN, když oba trasy jsou hello stejné, použije Azure hello nejdelší předponu shodu toochoose hello trasy směrem cíle hello paketu.</span><span class="sxs-lookup"><span data-stu-id="b270c-137">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are hello same, Azure will use hello longest prefix match toochoose hello route towards hello packet's destination.</span></span>
> 
> 

![Současná existence](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a><span data-ttu-id="b270c-139">Konfigurace Site-to-Site VPN tooconnect toosites nejsou připojené prostřednictvím ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="b270c-139">Configure a Site-to-Site VPN tooconnect toosites not connected through ExpressRoute</span></span>
<span data-ttu-id="b270c-140">Můžete nakonfigurovat síti, kde některé weby jsou připojené přímo tooAzure prostřednictvím sítě Site-to-Site VPN a některé weby přes ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b270c-140">You can configure your network where some sites connect directly tooAzure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Současná existence](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="b270c-142">Virtuální síť nemůžete nakonfigurovat jako směrovač provozu.</span><span class="sxs-lookup"><span data-stu-id="b270c-142">You cannot a configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-hello-steps-toouse"></a><span data-ttu-id="b270c-143">Výběr toouse kroky hello</span><span class="sxs-lookup"><span data-stu-id="b270c-143">Selecting hello steps toouse</span></span>
<span data-ttu-id="b270c-144">Existují dvě sady postupů toochoose z v pořadí tooconfigure připojení, které mohou existovat vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="b270c-144">There are two different sets of procedures toochoose from in order tooconfigure connections that can coexist.</span></span> <span data-ttu-id="b270c-145">Postup konfigurace Hello, který vyberete, bude záviset na tom, jestli máte existující virtuální síť, které mají tooconnect k, nebo chcete toocreate nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b270c-145">hello configuration procedure that you select will depend on whether you have an existing virtual network that you want tooconnect to, or you want toocreate a new virtual network.</span></span>

* <span data-ttu-id="b270c-146">Nemám virtuální síť a potřebuji toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="b270c-146">I don't have a VNet and need toocreate one.</span></span>
  
    <span data-ttu-id="b270c-147">Pokud ještě nemáte virtuální síť, tento postup vás provede vytvořením nové virtuální sítě pomocí modelu nasazení classic hello a vytvoření nových připojení ExpressRoute a Site-to-Site VPN.</span><span class="sxs-lookup"><span data-stu-id="b270c-147">If you don’t already have a virtual network, this procedure will walk you through creating a new virtual network using hello classic deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="b270c-148">tooconfigure, postupujte podle kroků hello v části článku hello [toocreate nové virtuální sítě a koexistujících připojení](#new).</span><span class="sxs-lookup"><span data-stu-id="b270c-148">tooconfigure, follow hello steps in hello article section [toocreate a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="b270c-149">Už mám virtuální síť modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="b270c-149">I already have a classic deployment model VNet.</span></span>
  
    <span data-ttu-id="b270c-150">Už můžete mít virtuální síť s existujícím připojením VPN typu site-to-site nebo připojením ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b270c-150">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="b270c-151">Hello části [tooconfigure koexistujících připojení pro už existující virtuální síť](#add) vás provede procesem odstranění hello brány a následného vytvoření nových připojení ExpressRoute a VPN typu Site-to-Site.</span><span class="sxs-lookup"><span data-stu-id="b270c-151">hello article section [tooconfigure coexsiting connections for an already existing VNet](#add) will walk you through deleting hello gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="b270c-152">Všimněte si, že při vytváření hello nová připojení, musí hello kroky provedené ve velmi specifickém pořadí.</span><span class="sxs-lookup"><span data-stu-id="b270c-152">Note that when creating hello new connections, hello steps must be completed in a very specific order.</span></span> <span data-ttu-id="b270c-153">Nepoužívejte hello pokyny v jiných článcích toocreate připojení a bran.</span><span class="sxs-lookup"><span data-stu-id="b270c-153">Don't use hello instructions in other articles toocreate your gateways and connections.</span></span>
  
    <span data-ttu-id="b270c-154">V tomto postupu vytvoření připojení, která mohou existovat vedle sebe vyžadují toodelete můžete bránu a pak nakonfigurovali nové brány.</span><span class="sxs-lookup"><span data-stu-id="b270c-154">In this procedure, creating connections that can coexist will require you toodelete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="b270c-155">To znamená, že budete mít výpadku pro připojení mezi různými místy odstranit a znovu vytvořte brány a připojení, ale nebude nutné toomigrate všechny virtuální počítače a služby tooa nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b270c-155">This means you will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need toomigrate any of your VMs or services tooa new virtual network.</span></span> <span data-ttu-id="b270c-156">Virtuální počítače a služby bude nadále možné toocommunicate se prostřednictvím nástroje pro vyrovnávání zatížení hello během konfigurace brány, pokud jsou nakonfigurované toodo tak.</span><span class="sxs-lookup"><span data-stu-id="b270c-156">Your VMs and services will still be able toocommunicate out through hello load balancer while you configure your gateway if they are configured toodo so.</span></span>

## <span data-ttu-id="b270c-157"><a name="new"></a>toocreate nové virtuální sítě a koexistujících připojení</span><span class="sxs-lookup"><span data-stu-id="b270c-157"><a name="new"></a>toocreate a new virtual network and coexisting connections</span></span>
<span data-ttu-id="b270c-158">Tento postup vás provede procesem vytvoření virtuální sítě a vytvoření připojení ExpressRoute a VPN site-to-site, která budou existovat společně.</span><span class="sxs-lookup"><span data-stu-id="b270c-158">This procedure will walk you through creating a VNet and create Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="b270c-159">Budete potřebovat tooinstall hello nejnovější verzi rutin prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-159">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="b270c-160">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-160">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span> <span data-ttu-id="b270c-161">Všimněte si, že může být hello rutin, které budete používat pro tuto konfiguraci poněkud liší od co je znají.</span><span class="sxs-lookup"><span data-stu-id="b270c-161">Note that hello cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="b270c-162">Být jisti toouse hello rutiny určené v těchto pokynech.</span><span class="sxs-lookup"><span data-stu-id="b270c-162">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="b270c-163">Vytvořte schéma pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b270c-163">Create a schema for your virtual network.</span></span> <span data-ttu-id="b270c-164">Další informace o schématu konfigurace hello najdete v tématu [schéma konfigurace Azure Virtual Network](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="b270c-164">For more information about hello configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   
    <span data-ttu-id="b270c-165">Při vytváření schématu, ujistěte se, že používáte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b270c-165">When you create your schema, make sure you use hello following values:</span></span>
   
   * <span data-ttu-id="b270c-166">Hello podsíť brány pro virtuální síť hello musí být/27 nebo kratší předpona (například/26 nebo /25).</span><span class="sxs-lookup"><span data-stu-id="b270c-166">hello gateway subnet for hello virtual network must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   * <span data-ttu-id="b270c-167">Typ připojení brány Hello je "Dedicated".</span><span class="sxs-lookup"><span data-stu-id="b270c-167">hello gateway connection type is "Dedicated".</span></span>
     
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
3. <span data-ttu-id="b270c-168">Po vytvoření a konfiguraci souboru schématu xml, nahrajte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-168">After creating and configuring your xml schema file, upload hello file.</span></span> <span data-ttu-id="b270c-169">Tím vytvoříte virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="b270c-169">This will create your virtual network.</span></span>
   
    <span data-ttu-id="b270c-170">Pomocí následující rutiny tooupload hello souboru, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="b270c-170">Use hello following cmdlet tooupload your file, replacing hello value with your own.</span></span>
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <span data-ttu-id="b270c-171"><a name="gw"></a>Vytvořte bránu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b270c-171"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="b270c-172">Být jisti toospecify hello GatewaySKU jako *standardní*, *HighPerformance*, nebo *UltraPerformance* a hello GatewayType jako *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="b270c-172">Be sure toospecify hello GatewaySKU as *Standard*, *HighPerformance*, or *UltraPerformance* and hello GatewayType as *DynamicRouting*.</span></span>
   
    <span data-ttu-id="b270c-173">Použijte hello následující ukázka, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="b270c-173">Use hello following sample, substituting hello values for your own.</span></span>
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. <span data-ttu-id="b270c-174">Propojení okruhu ExpressRoute toohello brány ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-174">Link hello ExpressRoute gateway toohello ExpressRoute circuit.</span></span> <span data-ttu-id="b270c-175">Po dokončení tohoto kroku se hello připojení mezi místní sítí a Azure prostřednictvím ExpressRoute vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="b270c-175">After this step has been completed, hello connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span>
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <span data-ttu-id="b270c-176"><a name="vpngw"></a>Dále vytvořte bránu VPN typu site-to-site.</span><span class="sxs-lookup"><span data-stu-id="b270c-176"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="b270c-177">Hello GatewaySKU musí být *standardní*, *HighPerformance*, nebo *UltraPerformance* a hello GatewayType musí být *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="b270c-177">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance* and hello GatewayType must be *DynamicRouting*.</span></span>
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    <span data-ttu-id="b270c-178">tooretrieve hello nastavení brány virtuální sítě, včetně ID brány hello a hello veřejné IP adresy, použijte hello `Get-AzureVirtualNetworkGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="b270c-178">tooretrieve hello virtual network gateway settings, including hello gateway ID and hello public IP, use hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span>
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for hello following virtual network: GNSDesMoines
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
7. <span data-ttu-id="b270c-179">Vytvořte entitu brány VPN místního webu.</span><span class="sxs-lookup"><span data-stu-id="b270c-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="b270c-180">Tento příkaz neprovede konfiguraci vaší místní brány VPN.</span><span class="sxs-lookup"><span data-stu-id="b270c-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="b270c-181">Místo toho můžete nastavení místní brány hello tooprovide, jako je například veřejná IP adresa hello a hello místní adresní prostor, aby hello Azure VPN gateway bude moct připojit tooit.</span><span class="sxs-lookup"><span data-stu-id="b270c-181">Rather, it allows you tooprovide hello local gateway settings, such as hello public IP and hello on-premises address space, so that hello Azure VPN gateway can connect tooit.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="b270c-182">místní web Hello hello Site-to-Site VPN není definován v hello netcfg.</span><span class="sxs-lookup"><span data-stu-id="b270c-182">hello local site for hello Site-to-Site VPN is not defined in hello netcfg.</span></span> <span data-ttu-id="b270c-183">Místo toho musíte použít tento parametry rutiny toospecify hello místního webu.</span><span class="sxs-lookup"><span data-stu-id="b270c-183">Instead, you must use this cmdlet toospecify hello local site parameters.</span></span> <span data-ttu-id="b270c-184">Nelze definovat pomocí portálu nebo souboru netcfg hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-184">You cannot define it using either portal, or hello netcfg file.</span></span>
   > 
   > 
   
    <span data-ttu-id="b270c-185">Použijte hello následující ukázka, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="b270c-185">Use hello following sample, replacing hello values with your own.</span></span>
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > <span data-ttu-id="b270c-186">Pokud vaše místní síť obsahuje víc tras, můžete je předat všechny najednou jako pole.</span><span class="sxs-lookup"><span data-stu-id="b270c-186">If your local network has multiple routes, you can pass them all in as an array.</span></span>  <span data-ttu-id="b270c-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span><span class="sxs-lookup"><span data-stu-id="b270c-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span></span>  
   > 
   > 

    <span data-ttu-id="b270c-188">tooretrieve hello nastavení brány virtuální sítě, včetně ID brány hello a hello veřejné IP adresy, použijte hello `Get-AzureVirtualNetworkGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="b270c-188">tooretrieve hello virtual network gateway settings, including hello gateway ID and hello public IP, use hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="b270c-189">Viz následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-189">See hello following example.</span></span>

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. <span data-ttu-id="b270c-190">Nakonfigurujte místní zařízení tooconnect toohello novou bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="b270c-190">Configure your local VPN device tooconnect toohello new gateway.</span></span> <span data-ttu-id="b270c-191">Použijte hello informace, které jste získali v kroku 6 při konfiguraci zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="b270c-191">Use hello information that you retrieved in step 6 when configuring your VPN device.</span></span> <span data-ttu-id="b270c-192">Další informace o konfiguraci zařízení VPN najdete v tématu [Konfigurace zařízení VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="b270c-192">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
2. <span data-ttu-id="b270c-193">Brána Site-to-Site VPN hello odkaz na Azure toohello místní brány.</span><span class="sxs-lookup"><span data-stu-id="b270c-193">Link hello Site-to-Site VPN gateway on Azure toohello local gateway.</span></span>
   
    <span data-ttu-id="b270c-194">V tomto příkladu je connectedEntityId hello ID místní brány, které můžete najít spuštěním `Get-AzureLocalNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="b270c-194">In this example, connectedEntityId is hello local gateway ID, which you can find by running `Get-AzureLocalNetworkGateway`.</span></span> <span data-ttu-id="b270c-195">VirtualNetworkGatewayId můžete najít pomocí hello `Get-AzureVirtualNetworkGateway` rutiny.</span><span class="sxs-lookup"><span data-stu-id="b270c-195">You can find virtualNetworkGatewayId by using hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="b270c-196">Po provedení tohoto kroku hello připojení mezi místní sítí a Azure prostřednictvím hello připojení Site-to-Site VPN je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="b270c-196">After this step, hello connection between your local network and Azure via hello Site-to-Site VPN connection is established.</span></span>

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <span data-ttu-id="b270c-197"><a name="add"></a>tooconfigure koexistujících připojení pro už existující virtuální síť</span><span class="sxs-lookup"><span data-stu-id="b270c-197"><a name="add"></a>tooconfigure coexsiting connections for an already existing VNet</span></span>
<span data-ttu-id="b270c-198">Pokud máte existující virtuální síť, zkontrolujte velikost podsítě brány hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-198">If you have an existing virtual network, check hello gateway subnet size.</span></span> <span data-ttu-id="b270c-199">Pokud podsíť brány hello velikosti/28 nebo/29, musíte nejprve odstranit bránu virtuální sítě hello a zvýšit velikost podsítě brány hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-199">If hello gateway subnet is /28 or /29, you must first delete hello virtual network gateway and increase hello gateway subnet size.</span></span> <span data-ttu-id="b270c-200">Hello kroky v této části se dozvíte, jak toodo který.</span><span class="sxs-lookup"><span data-stu-id="b270c-200">hello steps in this section will show you how toodo that.</span></span>

<span data-ttu-id="b270c-201">Pokud hello podsíť brány je/27 nebo větší a hello virtuální síť je připojená přes ExpressRoute, můžete přeskočit hello kroky a přejít příliš["Krok 6 – Vytvoření brány Site-to-Site VPN"](#vpngw) v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-201">If hello gateway subnet is /27 or larger and hello virtual network is connected via ExpressRoute, you can skip hello steps below and proceed too["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in hello previous section.</span></span>

> [!NOTE]
> <span data-ttu-id="b270c-202">Pokud odstraníte existující bránu hello, místní místo ztratí hello připojení tooyour virtuální sítě během práce na této konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b270c-202">When you delete hello existing gateway, your local premises will lose hello connection tooyour virtual network while you are working on this configuration.</span></span>
> 
> 

1. <span data-ttu-id="b270c-203">Budete potřebovat tooinstall hello nejnovější verzi hello rutiny Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b270c-203">You'll need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="b270c-204">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-204">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span> <span data-ttu-id="b270c-205">Všimněte si, že může být hello rutin, které budete používat pro tuto konfiguraci poněkud liší od co je znají.</span><span class="sxs-lookup"><span data-stu-id="b270c-205">Note that hello cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="b270c-206">Být jisti toouse hello rutiny určené v těchto pokynech.</span><span class="sxs-lookup"><span data-stu-id="b270c-206">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="b270c-207">Odstraňte existující bránu ExpressRoute nebo VPN typu Site-to-Site na hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-207">Delete hello existing ExpressRoute or Site-to-Site VPN gateway.</span></span> <span data-ttu-id="b270c-208">Použijte následující rutinu, nahraďte hello hodnoty vlastními hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-208">Use hello following cmdlet, replacing hello values with your own.</span></span>
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. <span data-ttu-id="b270c-209">Exportujte schéma virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-209">Export hello virtual network schema.</span></span> <span data-ttu-id="b270c-210">Použijte hello následující rutiny prostředí PowerShell, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="b270c-210">Use hello following PowerShell cmdlet, replacing hello values with your own.</span></span>
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. <span data-ttu-id="b270c-211">Upravte schéma konfiguračního souboru hello sítě tak, aby hello podsíť brány je/27 nebo kratší předpona (například/26 nebo /25).</span><span class="sxs-lookup"><span data-stu-id="b270c-211">Edit hello network configuration file schema so that hello gateway subnet is /27 or a shorter prefix (such as /26 or /25).</span></span> <span data-ttu-id="b270c-212">Viz následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="b270c-212">See hello following example.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="b270c-213">Pokud nemáte dost IP adres v velikost podsítě brány vaší virtuální sítě tooincrease hello, musíte tooadd další adresní prostor IP adres.</span><span class="sxs-lookup"><span data-stu-id="b270c-213">If you don't have enough IP addresses left in your virtual network tooincrease hello gateway subnet size, you need tooadd more IP address space.</span></span> <span data-ttu-id="b270c-214">Další informace o schématu konfigurace hello najdete v tématu [schéma konfigurace Azure Virtual Network](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="b270c-214">For more information about hello configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. <span data-ttu-id="b270c-215">Pokud předchozí brána byla VPN typu Site-to-Site, musíte změnit taky typ připojení hello příliš**Dedicated**.</span><span class="sxs-lookup"><span data-stu-id="b270c-215">If your previous gateway was a Site-to-Site VPN, you must also change hello connection type too**Dedicated**.</span></span>
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. <span data-ttu-id="b270c-216">V tuto chvíli máte virtuální síť, která nemá žádné brány.</span><span class="sxs-lookup"><span data-stu-id="b270c-216">At this point, you'll have a VNet with no gateways.</span></span> <span data-ttu-id="b270c-217">toocreate nové brány a dokončili připojení, abyste mohli pokračovat [krokem 4 – vytvoření brány ExpressRoute](#gw), který se nachází v hello předcházející sadě kroků.</span><span class="sxs-lookup"><span data-stu-id="b270c-217">toocreate new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in hello preceding set of steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b270c-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b270c-218">Next steps</span></span>
<span data-ttu-id="b270c-219">Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md)</span><span class="sxs-lookup"><span data-stu-id="b270c-219">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md)</span></span>

