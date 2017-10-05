---
title: "Plánování a návrhu pro připojení mezi různými místy: Azure VPN Gateway | Microsoft Docs"
description: "Další informace o službě VPN Gateway plánování a návrh mezi různými místy, hybridního i připojení VNet-to-VNet"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d5aaab83-4e74-4484-8bf0-cc465811e757
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2017
ms.author: cherylmc
ms.openlocfilehash: 0ebc3ef4a64432e993dd6ed69766bb64544fe433
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a><span data-ttu-id="63f9f-103">Plánování a návrh pro VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="63f9f-103">Planning and design for VPN Gateway</span></span>

<span data-ttu-id="63f9f-104">Plánování a návrhu mezi různými místy a VNet-to-VNet konfigurace může být jednoduchý nebo složitá, v závislosti na vašich sítí.</span><span class="sxs-lookup"><span data-stu-id="63f9f-104">Planning and designing your cross-premises and VNet-to-VNet configurations can be either simple, or complicated, depending on your networking needs.</span></span> <span data-ttu-id="63f9f-105">Tento článek vás provede základní aspekty plánování a návrhu.</span><span class="sxs-lookup"><span data-stu-id="63f9f-105">This article walks you through basic planning and design considerations.</span></span>

## <span data-ttu-id="63f9f-106"><a name="planning"></a>Plánování</span><span class="sxs-lookup"><span data-stu-id="63f9f-106"><a name="planning"></a>Planning</span></span>

### <span data-ttu-id="63f9f-107"><a name="compare"></a>Možnosti připojení mezi různými místy</span><span class="sxs-lookup"><span data-stu-id="63f9f-107"><a name="compare"></a>Cross-premises connectivity options</span></span>

<span data-ttu-id="63f9f-108">Pokud se chcete připojit místními servery bezpečně k virtuální síti, máte tři různé způsoby, jak udělat: Site-to-Site, Point-to-Site a ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="63f9f-108">If you want to connect your on-premises sites securely to a virtual network, you have three different ways to do so: Site-to-Site, Point-to-Site, and ExpressRoute.</span></span> <span data-ttu-id="63f9f-109">Porovnejte připojení různých mezi různými místy, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="63f9f-109">Compare the different cross-premises connections that are available.</span></span> <span data-ttu-id="63f9f-110">Možnosti, kterou zvolíte může záviset na různé aspekty, jako například:</span><span class="sxs-lookup"><span data-stu-id="63f9f-110">The option you choose can depend on various considerations, such as:</span></span>

* <span data-ttu-id="63f9f-111">Jaký druh propustnosti vyžaduje vaše řešení?</span><span class="sxs-lookup"><span data-stu-id="63f9f-111">What kind of throughput does your solution require?</span></span>
* <span data-ttu-id="63f9f-112">Chcete komunikovat přes veřejný internet prostřednictvím bezpečné VPN, nebo přes privátní připojení?</span><span class="sxs-lookup"><span data-stu-id="63f9f-112">Do you want to communicate over the public Internet via secure VPN, or over a private connection?</span></span>
* <span data-ttu-id="63f9f-113">Máte k dispozici veřejnou IP adresu?</span><span class="sxs-lookup"><span data-stu-id="63f9f-113">Do you have a public IP address available to use?</span></span>
* <span data-ttu-id="63f9f-114">Plánujete použít zařízení VPN?</span><span class="sxs-lookup"><span data-stu-id="63f9f-114">Are you planning to use a VPN device?</span></span> <span data-ttu-id="63f9f-115">Pokud ano, je kompatibilní?</span><span class="sxs-lookup"><span data-stu-id="63f9f-115">If so, is it compatible?</span></span>
* <span data-ttu-id="63f9f-116">Připojujete pouze několik počítačů, nebo chcete trvalé připojení pro svůj server?</span><span class="sxs-lookup"><span data-stu-id="63f9f-116">Are you connecting just a few computers, or do you want a persistent connection for your site?</span></span>
* <span data-ttu-id="63f9f-117">Jaký typ brány VPN vyžaduje řešení, které chcete vytvořit?</span><span class="sxs-lookup"><span data-stu-id="63f9f-117">What type of VPN gateway is required for the solution you want to create?</span></span>
* <span data-ttu-id="63f9f-118">Které skladová položka brány mám použít?</span><span class="sxs-lookup"><span data-stu-id="63f9f-118">Which gateway SKU should you use?</span></span>

### <span data-ttu-id="63f9f-119"><a name="planningtable"></a>Plánovací tabulka</span><span class="sxs-lookup"><span data-stu-id="63f9f-119"><a name="planningtable"></a>Planning table</span></span>

<span data-ttu-id="63f9f-120">V následující tabulce vám pomohou rozhodnout se nejlepší možnosti připojení pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="63f9f-120">The following table can help you decide the best connectivity option for your solution.</span></span>

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <span data-ttu-id="63f9f-121"><a name="gwsku"></a>SKU brány</span><span class="sxs-lookup"><span data-stu-id="63f9f-121"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <span data-ttu-id="63f9f-122"><a name="wf"></a>Pracovní postup</span><span class="sxs-lookup"><span data-stu-id="63f9f-122"><a name="wf"></a>Workflow</span></span>

<span data-ttu-id="63f9f-123">Následující seznam popisuje běžné pracovní postup pro připojení k síti cloudu:</span><span class="sxs-lookup"><span data-stu-id="63f9f-123">The following list outlines the common workflow for cloud connectivity:</span></span>

1. <span data-ttu-id="63f9f-124">Návrhu a plánování topologie připojení a vytvořte seznam adresních prostorů pro všechny sítě, které se chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="63f9f-124">Design and plan your connectivity topology and list the address spaces for all networks you want to connect.</span></span>
2. <span data-ttu-id="63f9f-125">Vytvoření virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="63f9f-125">Create an Azure virtual network.</span></span> 
3. <span data-ttu-id="63f9f-126">Vytvořte bránu VPN virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="63f9f-126">Create a VPN gateway for the virtual network.</span></span>
4. <span data-ttu-id="63f9f-127">Vytvořit a nakonfigurovat připojení k místní sítě nebo jiných virtuálních sítí (podle potřeby).</span><span class="sxs-lookup"><span data-stu-id="63f9f-127">Create and configure connections to on-premises networks or other virtual networks (as needed).</span></span>
5. <span data-ttu-id="63f9f-128">Vytvořit a nakonfigurovat připojení Point-to-Site pro bránu Azure VPN (podle potřeby).</span><span class="sxs-lookup"><span data-stu-id="63f9f-128">Create and configure a Point-to-Site connection for your Azure VPN gateway (as needed).</span></span>

## <span data-ttu-id="63f9f-129"><a name="design"></a>Návrh</span><span class="sxs-lookup"><span data-stu-id="63f9f-129"><a name="design"></a>Design</span></span>
### <span data-ttu-id="63f9f-130"><a name="topologies"></a>Topologie připojení ke službě</span><span class="sxs-lookup"><span data-stu-id="63f9f-130"><a name="topologies"></a>Connection topologies</span></span>

<span data-ttu-id="63f9f-131">Začít hledáním v diagramech [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) článku.</span><span class="sxs-lookup"><span data-stu-id="63f9f-131">Start by looking at the diagrams in the [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span> <span data-ttu-id="63f9f-132">Článek obsahuje základní diagramy, modely nasazení pro každý topologie a nástroje pro nasazení k dispozici, které můžete použít k nasazení vaší konfigurace.</span><span class="sxs-lookup"><span data-stu-id="63f9f-132">The article contains basic diagrams, the deployment models for each topology, and the available deployment tools you can use to deploy your configuration.</span></span>

### <span data-ttu-id="63f9f-133"><a name="designbasics"></a>Základní informace o návrhu</span><span class="sxs-lookup"><span data-stu-id="63f9f-133"><a name="designbasics"></a>Design basics</span></span>

<span data-ttu-id="63f9f-134">Následující části popisují základy brány VPN.</span><span class="sxs-lookup"><span data-stu-id="63f9f-134">The following sections discuss the VPN gateway basics.</span></span> 

#### <span data-ttu-id="63f9f-135"><a name="servicelimits"></a>Omezení služby sítě</span><span class="sxs-lookup"><span data-stu-id="63f9f-135"><a name="servicelimits"></a>Networking services limits</span></span>

<span data-ttu-id="63f9f-136">Procházení tabulky, které chcete zobrazit [sítě služby omezení](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="63f9f-136">Scroll through the tables to view [networking services limits](../azure-subscription-service-limits.md#networking-limits).</span></span> <span data-ttu-id="63f9f-137">Omezení uvedené může mít vliv na návrh vašeho.</span><span class="sxs-lookup"><span data-stu-id="63f9f-137">The limits listed may impact your design.</span></span>

#### <span data-ttu-id="63f9f-138"><a name="subnets"></a>O podsítě</span><span class="sxs-lookup"><span data-stu-id="63f9f-138"><a name="subnets"></a>About subnets</span></span>

<span data-ttu-id="63f9f-139">Při vytváření připojení, musíte zvážit vaší rozsahy podsítě.</span><span class="sxs-lookup"><span data-stu-id="63f9f-139">When you are creating connections, you must consider your subnet ranges.</span></span> <span data-ttu-id="63f9f-140">Nemůže mít překrývající se rozsahy adres podsítě.</span><span class="sxs-lookup"><span data-stu-id="63f9f-140">You cannot have overlapping subnet address ranges.</span></span> <span data-ttu-id="63f9f-141">Překrývající se podsítí je stejné adresní prostor, který obsahuje jiné umístění obsahuje jednu virtuální síť nebo místní umístění.</span><span class="sxs-lookup"><span data-stu-id="63f9f-141">An overlapping subnet is when one virtual network or on-premises location contains the same address space that the other location contains.</span></span> <span data-ttu-id="63f9f-142">To znamená, je nutné, aby technici sítě pro vaše místní sítě do vyčlenit oblasti, budete moci použít pro Azure na IP adresování místa nebo podsítě.</span><span class="sxs-lookup"><span data-stu-id="63f9f-142">This means that you need your network engineers for your local on-premises networks to carve out a range for you to use for your Azure IP addressing space/subnets.</span></span> <span data-ttu-id="63f9f-143">Je nutné adresní prostor, který se nepoužívá v místní síti.</span><span class="sxs-lookup"><span data-stu-id="63f9f-143">You need address space that is not being used on the local on-premises network.</span></span>

<span data-ttu-id="63f9f-144">Zamezení překrývající se podsítí je také důležité, když pracujete s připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="63f9f-144">Avoiding overlapping subnets is also important when you are working with VNet-to-VNet connections.</span></span> <span data-ttu-id="63f9f-145">Pokud podsítě překrývajících se a IP adresu v odesílání i cílové virtuální sítě existuje, připojení VNet-to-VNet se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="63f9f-145">If your subnets overlap and an IP address exists in both the sending and destination VNets, VNet-to-VNet connections fail.</span></span> <span data-ttu-id="63f9f-146">Azure nelze směrovat data do jiné virtuální sítě, protože cílová adresa je součástí odesílání virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="63f9f-146">Azure can't route the data to the other VNet because the destination address is part of the sending VNet.</span></span>

<span data-ttu-id="63f9f-147">Brány sítě VPN vyžaduje konkrétní podsíť s názvem podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="63f9f-147">VPN Gateways require a specific subnet called a gateway subnet.</span></span> <span data-ttu-id="63f9f-148">Pro správné fungování všech podsítí brány je nutné, aby měly název GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="63f9f-148">All gateway subnets must be named GatewaySubnet to work properly.</span></span> <span data-ttu-id="63f9f-149">Ujistěte se, aby název podsítě brány jiný název a podsíti brány nenasazujte virtuální počítače nebo cokoliv jiného.</span><span class="sxs-lookup"><span data-stu-id="63f9f-149">Be sure not to name your gateway subnet a different name, and don't deploy VMs or anything else to the gateway subnet.</span></span> <span data-ttu-id="63f9f-150">V tématu [podsítě brány](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span><span class="sxs-lookup"><span data-stu-id="63f9f-150">See [Gateway Subnets](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span></span>

#### <span data-ttu-id="63f9f-151"><a name="local"></a>O brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="63f9f-151"><a name="local"></a>About local network gateways</span></span>

<span data-ttu-id="63f9f-152">Brána místní sítě obvykle odkazuje na vaše místní umístění.</span><span class="sxs-lookup"><span data-stu-id="63f9f-152">The local network gateway typically refers to your on-premises location.</span></span> <span data-ttu-id="63f9f-153">V modelu nasazení classic bránu místní sítě se označuje jako místní síťové lokality.</span><span class="sxs-lookup"><span data-stu-id="63f9f-153">In the classic deployment model, the local network gateway is referred to as a Local Network Site.</span></span> <span data-ttu-id="63f9f-154">Při konfiguraci brány místní sítě, zadejte jeho název, zadejte veřejnou IP adresu místního zařízení VPN a zadáte předpony adres, které se nacházejí v místní umístění.</span><span class="sxs-lookup"><span data-stu-id="63f9f-154">When you configure a local network gateway, you give it a name, specify the public IP address of the on-premises VPN device, and specify the address prefixes that are in the on-premises location.</span></span> <span data-ttu-id="63f9f-155">Azure zjistí předpony cílových adres pro síťový provoz, zajímají konfiguraci, která jste zadali pro bránu místní sítě a směruje pakety odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="63f9f-155">Azure looks at the destination address prefixes for network traffic, consults the configuration that you have specified for the local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="63f9f-156">Předpony adres můžete upravit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="63f9f-156">You can modify the address prefixes as needed.</span></span> <span data-ttu-id="63f9f-157">Další informace najdete v tématu [brány místní sítě](vpn-gateway-about-vpn-gateway-settings.md#lng).</span><span class="sxs-lookup"><span data-stu-id="63f9f-157">For more information, see [Local network gateways](vpn-gateway-about-vpn-gateway-settings.md#lng).</span></span>

#### <span data-ttu-id="63f9f-158"><a name="gwtype"></a>O typech brány</span><span class="sxs-lookup"><span data-stu-id="63f9f-158"><a name="gwtype"></a>About gateway types</span></span>

<span data-ttu-id="63f9f-159">Výběr typu správné brány pro vaše topologie je velmi důležité.</span><span class="sxs-lookup"><span data-stu-id="63f9f-159">Selecting the correct gateway type for your topology is critical.</span></span> <span data-ttu-id="63f9f-160">Pokud vyberete chybný typ, brána nebude fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="63f9f-160">If you select the wrong type, your gateway won't work properly.</span></span> <span data-ttu-id="63f9f-161">Typ brány určuje, jak se brána samotná připojuje, a jedná se o požadované nastavení pro model nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="63f9f-161">The gateway type specifies how the gateway itself connects and is a required configuration setting for the Resource Manager deployment model.</span></span>

<span data-ttu-id="63f9f-162">Typy brány jsou:</span><span class="sxs-lookup"><span data-stu-id="63f9f-162">The gateway types are:</span></span>

* <span data-ttu-id="63f9f-163">Vpn</span><span class="sxs-lookup"><span data-stu-id="63f9f-163">Vpn</span></span>
* <span data-ttu-id="63f9f-164">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63f9f-164">ExpressRoute</span></span>

#### <span data-ttu-id="63f9f-165"><a name="connectiontype"></a>O typech připojení</span><span class="sxs-lookup"><span data-stu-id="63f9f-165"><a name="connectiontype"></a>About connection types</span></span>

<span data-ttu-id="63f9f-166">Každá konfigurace vyžaduje určitý typ připojení.</span><span class="sxs-lookup"><span data-stu-id="63f9f-166">Each configuration requires a specific connection type.</span></span> <span data-ttu-id="63f9f-167">Typy připojení jsou:</span><span class="sxs-lookup"><span data-stu-id="63f9f-167">The connection types are:</span></span>

* <span data-ttu-id="63f9f-168">Protokol IPsec</span><span class="sxs-lookup"><span data-stu-id="63f9f-168">IPsec</span></span>
* <span data-ttu-id="63f9f-169">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="63f9f-169">Vnet2Vnet</span></span>
* <span data-ttu-id="63f9f-170">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="63f9f-170">ExpressRoute</span></span>
* <span data-ttu-id="63f9f-171">VPNClient</span><span class="sxs-lookup"><span data-stu-id="63f9f-171">VPNClient</span></span>

#### <span data-ttu-id="63f9f-172"><a name="vpntype"></a>O typy sítě VPN</span><span class="sxs-lookup"><span data-stu-id="63f9f-172"><a name="vpntype"></a>About VPN types</span></span>

<span data-ttu-id="63f9f-173">Každá konfigurace vyžaduje určitý typ sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="63f9f-173">Each configuration requires a specific VPN type.</span></span> <span data-ttu-id="63f9f-174">Pokud kombinujete dvě konfigurace, například vytváříte-li připojení typu Site-to-Site a Point-to-Site ke stejné virtuální síti, musíte použít typ sítě VPN, který splňuje požadavky obou připojení.</span><span class="sxs-lookup"><span data-stu-id="63f9f-174">If you are combining two configurations, such as creating a Site-to-Site connection and a Point-to-Site connection to the same VNet, you must use a VPN type that satisfies both connection requirements.</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="63f9f-175">Následující tabulky popisují typ sítě VPN, jak se mapuje na každé konfiguraci připojení.</span><span class="sxs-lookup"><span data-stu-id="63f9f-175">The following tables show the VPN type as it maps to each connection configuration.</span></span> <span data-ttu-id="63f9f-176">Ujistěte se, že typ sítě VPN pro bránu odpovídá konfiguraci, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="63f9f-176">Make sure the VPN type for your gateway matches the configuration that you want to create.</span></span> 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <span data-ttu-id="63f9f-177"><a name="devices"></a>Zařízení VPN pro připojení Site-to-Site</span><span class="sxs-lookup"><span data-stu-id="63f9f-177"><a name="devices"></a>VPN devices for Site-to-Site connections</span></span>

<span data-ttu-id="63f9f-178">Konfigurace připojení Site-to-Site, bez ohledu na modelu nasazení, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="63f9f-178">To configure a Site-to-Site connection, regardless of deployment model, you need the following items:</span></span>

* <span data-ttu-id="63f9f-179">Zařízení VPN, který je kompatibilní s Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="63f9f-179">A VPN device that is compatible with Azure VPN gateways</span></span>
* <span data-ttu-id="63f9f-180">Veřejnou adresu IPv4 IP, který není za zařízení NAT</span><span class="sxs-lookup"><span data-stu-id="63f9f-180">A public-facing IPv4 IP address that is not behind a NAT</span></span>

<span data-ttu-id="63f9f-181">Budete muset prostředí konfiguraci zařízení VPN nebo někdo, která můžete nakonfigurovat zařízení pro vás k dispozici.</span><span class="sxs-lookup"><span data-stu-id="63f9f-181">You need to have experience configuring your VPN device, or have someone that can configure the device for you.</span></span>

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <span data-ttu-id="63f9f-182"><a name="forcedtunnel"></a>Vezměte v úvahu vynucené tunelového propojení směrování</span><span class="sxs-lookup"><span data-stu-id="63f9f-182"><a name="forcedtunnel"></a>Consider forced tunnel routing</span></span>

<span data-ttu-id="63f9f-183">Pro většinu konfiguraci můžete konfigurovat vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="63f9f-183">For most configurations, you can configure forced tunneling.</span></span> <span data-ttu-id="63f9f-184">Vynucené tunelování vám umožní přesměrování nebo "Vynutit" veškerý provoz vázaný na Internet zpět na místní umístění prostřednictvím tunelu Site-to-Site VPN pro kontrolu a auditování.</span><span class="sxs-lookup"><span data-stu-id="63f9f-184">Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="63f9f-185">Požadavek kritické zabezpečení pro většinu organizace IT zásad.</span><span class="sxs-lookup"><span data-stu-id="63f9f-185">This is a critical security requirement for most enterprise IT policies.</span></span> 

<span data-ttu-id="63f9f-186">Bez vynucené tunelování, se internetový provoz z virtuálních počítačů v Azure procházení od Azure síťové infrastruktury přímo se k Internetu, bez možnosti a umožní vám na svoji provoz vždy.</span><span class="sxs-lookup"><span data-stu-id="63f9f-186">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic.</span></span> <span data-ttu-id="63f9f-187">Neoprávněný přístup k Internetu může potenciálně vést k informacím nebo jiné typy narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="63f9f-187">Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.</span></span>

<span data-ttu-id="63f9f-188">V obou modelech nasazení a pomocí jiných nástrojů se dá konfigurovat vynucené tunelové připojení.</span><span class="sxs-lookup"><span data-stu-id="63f9f-188">A forced tunneling connection can be configured in both deployment models and by using different tools.</span></span> <span data-ttu-id="63f9f-189">Další informace najdete v tématu [konfigurace vynuceného tunelování](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="63f9f-189">For more information, see [Configure forced tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>

<span data-ttu-id="63f9f-190">**Vynutit tunelové diagram**</span><span class="sxs-lookup"><span data-stu-id="63f9f-190">**Forced tunneling diagram**</span></span>

![Vynutit tunelové diagram služby Azure VPN Gateway](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a><span data-ttu-id="63f9f-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63f9f-192">Next steps</span></span>

<span data-ttu-id="63f9f-193">Najdete v článku [VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md) a [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) články pro další informace vám pomohou při návrhu.</span><span class="sxs-lookup"><span data-stu-id="63f9f-193">See the [VPN Gateway FAQ](vpn-gateway-vpn-faq.md) and [About VPN Gateway](vpn-gateway-about-vpngateways.md) articles for more information to help you with your design.</span></span>

<span data-ttu-id="63f9f-194">Další informace o nastavení konkrétní brány najdete v tématu [o nastavení brány sítě VPN](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="63f9f-194">For more information about specific gateway settings, see [About VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>