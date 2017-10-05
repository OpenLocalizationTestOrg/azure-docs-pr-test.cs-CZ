---
title: "Přehled protokolu BGP se službou Azure VPN Gateways | Microsoft Docs"
description: "Tento článek obsahuje přehled protokolu BGP se službou Azure VPN Gateways."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: f8c3985c-c128-4f34-835c-0e88742bf36e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: 60d8dd45ecbd4a075721c25acadb21d4e2fd5448
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a><span data-ttu-id="ce70c-103">Přehled protokolu BGP se službou Azure VPN Gateways</span><span class="sxs-lookup"><span data-stu-id="ce70c-103">Overview of BGP with Azure VPN Gateways</span></span>
<span data-ttu-id="ce70c-104">Tento článek obsahuje přehled o podpoře protokolu BGP (Border Gateway Protocol) ve službě Azure VPN Gateways.</span><span class="sxs-lookup"><span data-stu-id="ce70c-104">This article provides an overview of BGP (Border Gateway Protocol) support in Azure VPN Gateways.</span></span>

<span data-ttu-id="ce70c-105">BGP je standardní směrovací protokol, na internetu běžně používaný k výměně informací o směrování a dostupnosti mezi dvěma nebo více sítěmi.</span><span class="sxs-lookup"><span data-stu-id="ce70c-105">BGP is the standard routing protocol commonly used in the Internet to exchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="ce70c-106">Pokud protokol BGP použijete v rámci virtuálních sítí Azure, umožní službám Azure VPN Gateway a místním zařízením VPN, která se nazývají partnerské uzly protokolu BGP nebo sousedé BGP, výměnu „tras“ informujících obě brány o dostupnosti a dosažitelnosti předpon, které procházejí těmito bránami nebo trasami.</span><span class="sxs-lookup"><span data-stu-id="ce70c-106">When used in the context of Azure Virtual Networks, BGP enables the Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, to exchange "routes" that will inform both gateways on the availability and reachability for those prefixes to go through the gateways or routers involved.</span></span> <span data-ttu-id="ce70c-107">Protokol BGP také umožňuje směrování přenosu mezi více sítěmi pomocí šíření tras, které brána s protokolem BGP zjistí od jednoho partnerského uzlu protokolu BGP, do všech dalších partnerských uzlů protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="ce70c-107">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer to all other BGP peers.</span></span> 

## <a name="why-use-bgp"></a><span data-ttu-id="ce70c-108">Proč používat protokol BGP?</span><span class="sxs-lookup"><span data-stu-id="ce70c-108">Why use BGP?</span></span>
<span data-ttu-id="ce70c-109">Protokol BGP je volitelná funkce, kterou můžete použít s trasovými bránami Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="ce70c-109">BGP is an optional feature you can use with Azure Route-Based VPN gateways.</span></span> <span data-ttu-id="ce70c-110">Dříve než povolíte tuto funkci byste se měli ujistit, že vaše místní zařízení VPN podporuje protokol BGP.</span><span class="sxs-lookup"><span data-stu-id="ce70c-110">You should also make sure your on-premises VPN devices support BGP before you enable the feature.</span></span> <span data-ttu-id="ce70c-111">Brány Azure VPN a místní zařízení VPN můžete nadále používat i bez protokolu BGP.</span><span class="sxs-lookup"><span data-stu-id="ce70c-111">You can continue to use Azure VPN gateways and your on-premises VPN devices without BGP.</span></span> <span data-ttu-id="ce70c-112">Je to stejné jako používání statických tras (bez protokolu BGP) *oproti* používání dynamického směrování s protokolem BGP mezi vaší sítí a Azure.</span><span class="sxs-lookup"><span data-stu-id="ce70c-112">It is the equivalent of using static routes (without BGP) *vs.* using dynamic routing with BGP between your networks and Azure.</span></span>

<span data-ttu-id="ce70c-113">Existuje několik výhod a nových schopností při použití protokolu BGP:</span><span class="sxs-lookup"><span data-stu-id="ce70c-113">There are several advantages and new capabilities with BGP:</span></span>

### <a name="support-automatic-and-flexible-prefix-updates"></a><span data-ttu-id="ce70c-114">Podpora automatických a flexibilních aktualizací předpon</span><span class="sxs-lookup"><span data-stu-id="ce70c-114">Support automatic and flexible prefix updates</span></span>
<span data-ttu-id="ce70c-115">U protokolu BGP musíte pouze deklarovat minimální předponu určitému partnerskému uzlu protokolu BGP přes tunel S2S VPN s protokolem IPsec.</span><span class="sxs-lookup"><span data-stu-id="ce70c-115">With BGP, you only need to declare a minimum prefix to a specific BGP peer over the IPsec S2S VPN tunnel.</span></span> <span data-ttu-id="ce70c-116">Ta může být malá jako předpona hostitele (/32) IP adresy partnerského uzlu protokolu BGP vašeho místního zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="ce70c-116">It can be as small as a host prefix (/32) of the BGP peer IP address of your on-premises VPN device.</span></span> <span data-ttu-id="ce70c-117">Můžete určit, které předpony místní sítě chcete inzerovat do Azure pro umožnění přístupu službě Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="ce70c-117">You can control which on-premises network prefixes you want to advertise to Azure to allow your Azure Virtual Network to access.</span></span>

<span data-ttu-id="ce70c-118">Můžete také inzerovat delší předpony, které mohou obsahovat některé předpony adres vaší virtuální sítě, například velký prostor privátní IP adresy (např. 10.0.0.0/8).</span><span class="sxs-lookup"><span data-stu-id="ce70c-118">You can also advertise larger prefixes that may include some of your VNet address prefixes, such as a large private IP address space (for example, 10.0.0.0/8).</span></span> <span data-ttu-id="ce70c-119">Poznámka: když předpony nesmí shodovat s žádnými předponami vaší virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ce70c-119">Note though the prefixes cannot be identical with any one of your VNet prefixes.</span></span> <span data-ttu-id="ce70c-120">Trasy shodné s předponami vaší virtuální sítě budou odmítnuty.</span><span class="sxs-lookup"><span data-stu-id="ce70c-120">Those routes identical to your VNet prefixes will be rejected.</span></span>

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a><span data-ttu-id="ce70c-121">Podpora více tunelů mezi virtuální sítí a místním webem s automatickým převzetím služeb při selhání na základě protokolu BGP</span><span class="sxs-lookup"><span data-stu-id="ce70c-121">Support multiple tunnels between a VNet and an on-premises site with automatic failover based on BGP</span></span>
<span data-ttu-id="ce70c-122">Můžete vytvořit více připojení mezi virtuální sítí Azure a místními zařízeními VPN ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="ce70c-122">You can establish multiple connections between your Azure VNet and your on-premises VPN devices in the same location.</span></span> <span data-ttu-id="ce70c-123">Tato schopnost poskytuje více tunelů (cest) mezi těmito dvěma sítěmi v konfiguraci aktivní-aktivní.</span><span class="sxs-lookup"><span data-stu-id="ce70c-123">This capability provides multiple tunnels (paths) between the two networks in an active-active configuration.</span></span> <span data-ttu-id="ce70c-124">Pokud je odpojení jednoho tunelu, odpovídající trasy se odvolají přes protokol BGP a provoz se automaticky přesouvá na zbývající tunely.</span><span class="sxs-lookup"><span data-stu-id="ce70c-124">If one of the tunnels is disconnected, the corresponding routes will be withdrawn via BGP and the traffic automatically shifts to the remaining tunnels.</span></span>

<span data-ttu-id="ce70c-125">Následující diagram ukazuje jednoduchý příklad tohoto vysoce dostupného nastavení:</span><span class="sxs-lookup"><span data-stu-id="ce70c-125">The following diagram shows a simple example of this highly available setup:</span></span>

![Více aktivních cest](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a><span data-ttu-id="ce70c-127">Podpora směrování přenosu mezi místními sítěmi a více virtuálními sítěmi Azure</span><span class="sxs-lookup"><span data-stu-id="ce70c-127">Support transit routing between your on-premises networks and multiple Azure VNets</span></span>
<span data-ttu-id="ce70c-128">Protokol BGP umožňuje více branám zjišťovat a šířit předpony z různých sítí, ať jsou připojeny přímo nebo nepřímo.</span><span class="sxs-lookup"><span data-stu-id="ce70c-128">BGP enables multiple gateways to learn and propagate prefixes from different networks, whether they are directly or indirectly connected.</span></span> <span data-ttu-id="ce70c-129">To umožňuje směrování přenosu pomocí bran Azure VPN mezi vašimi místními weby nebo napříč více službami Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="ce70c-129">This can enable transit routing with Azure VPN gateways between your on-premises sites or across multiple Azure Virtual Networks.</span></span>

<span data-ttu-id="ce70c-130">Následující diagram ukazuje příklad topologie vícenásobného předávání s více cestami, které mohou přenášet provoz mezi dvěma místními sítěmi přes brány Azure VPN v rámci služby MSN:</span><span class="sxs-lookup"><span data-stu-id="ce70c-130">The following diagram shows an example of a multi-hop topology with multiple paths that can transit traffic between the two on-premises networks through Azure VPN gateways within the Microsoft Networks:</span></span>

![Vícenásobné předávání přenosu](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a><span data-ttu-id="ce70c-132">PROTOKOL BGP – NEJČASTĚJŠÍ DOTAZY</span><span class="sxs-lookup"><span data-stu-id="ce70c-132">BGP FAQ</span></span>
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="ce70c-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce70c-133">Next steps</span></span>
<span data-ttu-id="ce70c-134">V tématu [Začínáme s protokolem BGP na branách Azure VPN](vpn-gateway-bgp-resource-manager-ps.md) najdete postup konfigurace protokolu BGP pro připojení mezi různými místy a pro připojení mezi virtuálními sítěmi (VNet-to-VNet).</span><span class="sxs-lookup"><span data-stu-id="ce70c-134">See [Getting started with BGP on Azure VPN gateways](vpn-gateway-bgp-resource-manager-ps.md) for steps to configure BGP for your cross-premises and VNet-to-VNet connections.</span></span>

