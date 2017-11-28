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
ms.openlocfilehash: 3d4587ba31d163384212eca88a7e2c0ba8f3b21f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a><span data-ttu-id="5a338-103">Plánování a návrh pro VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="5a338-103">Planning and design for VPN Gateway</span></span>

<span data-ttu-id="5a338-104">Plánování a návrhu mezi různými místy a VNet-to-VNet konfigurace může být jednoduchý nebo složitá, v závislosti na vašich sítí.</span><span class="sxs-lookup"><span data-stu-id="5a338-104">Planning and designing your cross-premises and VNet-to-VNet configurations can be either simple, or complicated, depending on your networking needs.</span></span> <span data-ttu-id="5a338-105">Tento článek vás provede základní aspekty plánování a návrhu.</span><span class="sxs-lookup"><span data-stu-id="5a338-105">This article walks you through basic planning and design considerations.</span></span>

## <span data-ttu-id="5a338-106"><a name="planning"></a>Plánování</span><span class="sxs-lookup"><span data-stu-id="5a338-106"><a name="planning"></a>Planning</span></span>

### <span data-ttu-id="5a338-107"><a name="compare"></a>Možnosti připojení mezi různými místy</span><span class="sxs-lookup"><span data-stu-id="5a338-107"><a name="compare"></a>Cross-premises connectivity options</span></span>

<span data-ttu-id="5a338-108">Pokud chcete tooconnect místní lokality bezpečně tooa virtuální sítě, takže máte tři různé způsoby toodo: Site-to-Site, Point-to-Site a ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="5a338-108">If you want tooconnect your on-premises sites securely tooa virtual network, you have three different ways toodo so: Site-to-Site, Point-to-Site, and ExpressRoute.</span></span> <span data-ttu-id="5a338-109">Porovnejte hello různých mezi různými místy připojení, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5a338-109">Compare hello different cross-premises connections that are available.</span></span> <span data-ttu-id="5a338-110">možnost Hello může záviset na různé aspekty, jako třeba:</span><span class="sxs-lookup"><span data-stu-id="5a338-110">hello option you choose can depend on various considerations, such as:</span></span>

* <span data-ttu-id="5a338-111">Jaký druh propustnosti vyžaduje vaše řešení?</span><span class="sxs-lookup"><span data-stu-id="5a338-111">What kind of throughput does your solution require?</span></span>
* <span data-ttu-id="5a338-112">Chcete toocommunicate přes hello veřejný Internet prostřednictvím bezpečné VPN nebo přes privátní připojení?</span><span class="sxs-lookup"><span data-stu-id="5a338-112">Do you want toocommunicate over hello public Internet via secure VPN, or over a private connection?</span></span>
* <span data-ttu-id="5a338-113">Máte veřejných IP adres k dispozici toouse?</span><span class="sxs-lookup"><span data-stu-id="5a338-113">Do you have a public IP address available toouse?</span></span>
* <span data-ttu-id="5a338-114">Pokud plánujete toouse zařízení VPN?</span><span class="sxs-lookup"><span data-stu-id="5a338-114">Are you planning toouse a VPN device?</span></span> <span data-ttu-id="5a338-115">Pokud ano, je kompatibilní?</span><span class="sxs-lookup"><span data-stu-id="5a338-115">If so, is it compatible?</span></span>
* <span data-ttu-id="5a338-116">Připojujete pouze několik počítačů, nebo chcete trvalé připojení pro svůj server?</span><span class="sxs-lookup"><span data-stu-id="5a338-116">Are you connecting just a few computers, or do you want a persistent connection for your site?</span></span>
* <span data-ttu-id="5a338-117">Jaký typ brány VPN je požadované pro řešení hello chcete toocreate?</span><span class="sxs-lookup"><span data-stu-id="5a338-117">What type of VPN gateway is required for hello solution you want toocreate?</span></span>
* <span data-ttu-id="5a338-118">Které skladová položka brány mám použít?</span><span class="sxs-lookup"><span data-stu-id="5a338-118">Which gateway SKU should you use?</span></span>

### <span data-ttu-id="5a338-119"><a name="planningtable"></a>Plánovací tabulka</span><span class="sxs-lookup"><span data-stu-id="5a338-119"><a name="planningtable"></a>Planning table</span></span>

<span data-ttu-id="5a338-120">Hello následující tabulka vám může pomoct rozhodnout hello nejlepší možnosti připojení pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="5a338-120">hello following table can help you decide hello best connectivity option for your solution.</span></span>

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <span data-ttu-id="5a338-121"><a name="gwsku"></a>SKU brány</span><span class="sxs-lookup"><span data-stu-id="5a338-121"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <span data-ttu-id="5a338-122"><a name="wf"></a>Pracovní postup</span><span class="sxs-lookup"><span data-stu-id="5a338-122"><a name="wf"></a>Workflow</span></span>

<span data-ttu-id="5a338-123">Hello následující seznam obsahuje přehled hello běžné pracovní postup pro připojení k cloudu:</span><span class="sxs-lookup"><span data-stu-id="5a338-123">hello following list outlines hello common workflow for cloud connectivity:</span></span>

1. <span data-ttu-id="5a338-124">Návrh a plánování, že vaše připojení topologie a seznamu Adresa hello prostory pro všechny sítě chcete tooconnect.</span><span class="sxs-lookup"><span data-stu-id="5a338-124">Design and plan your connectivity topology and list hello address spaces for all networks you want tooconnect.</span></span>
2. <span data-ttu-id="5a338-125">Vytvoření virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="5a338-125">Create an Azure virtual network.</span></span> 
3. <span data-ttu-id="5a338-126">Vytvořte bránu sítě VPN pro virtuální síť hello.</span><span class="sxs-lookup"><span data-stu-id="5a338-126">Create a VPN gateway for hello virtual network.</span></span>
4. <span data-ttu-id="5a338-127">Vytvořit a nakonfigurovat připojení tooon místní sítě nebo jiné virtuální sítě (podle potřeby).</span><span class="sxs-lookup"><span data-stu-id="5a338-127">Create and configure connections tooon-premises networks or other virtual networks (as needed).</span></span>
5. <span data-ttu-id="5a338-128">Vytvořit a nakonfigurovat připojení Point-to-Site pro bránu Azure VPN (podle potřeby).</span><span class="sxs-lookup"><span data-stu-id="5a338-128">Create and configure a Point-to-Site connection for your Azure VPN gateway (as needed).</span></span>

## <span data-ttu-id="5a338-129"><a name="design"></a>Návrh</span><span class="sxs-lookup"><span data-stu-id="5a338-129"><a name="design"></a>Design</span></span>
### <span data-ttu-id="5a338-130"><a name="topologies"></a>Topologie připojení ke službě</span><span class="sxs-lookup"><span data-stu-id="5a338-130"><a name="topologies"></a>Connection topologies</span></span>

<span data-ttu-id="5a338-131">Začněte tím, že prohlížení hello diagramů v hello [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5a338-131">Start by looking at hello diagrams in hello [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span> <span data-ttu-id="5a338-132">Hello článek obsahuje základní diagramy, hello modely nasazení pro každou topologie a nástroje pro nasazení k dispozici hello toodeploy můžete použít konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5a338-132">hello article contains basic diagrams, hello deployment models for each topology, and hello available deployment tools you can use toodeploy your configuration.</span></span>

### <span data-ttu-id="5a338-133"><a name="designbasics"></a>Základní informace o návrhu</span><span class="sxs-lookup"><span data-stu-id="5a338-133"><a name="designbasics"></a>Design basics</span></span>

<span data-ttu-id="5a338-134">Hello následující oddíly popisují základy brány VPN hello.</span><span class="sxs-lookup"><span data-stu-id="5a338-134">hello following sections discuss hello VPN gateway basics.</span></span> 

#### <span data-ttu-id="5a338-135"><a name="servicelimits"></a>Omezení služby sítě</span><span class="sxs-lookup"><span data-stu-id="5a338-135"><a name="servicelimits"></a>Networking services limits</span></span>

<span data-ttu-id="5a338-136">Procházení hello tabulky tooview [sítě služby omezení](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="5a338-136">Scroll through hello tables tooview [networking services limits](../azure-subscription-service-limits.md#networking-limits).</span></span> <span data-ttu-id="5a338-137">omezení Hello uvedené může mít vliv na návrh vašeho.</span><span class="sxs-lookup"><span data-stu-id="5a338-137">hello limits listed may impact your design.</span></span>

#### <span data-ttu-id="5a338-138"><a name="subnets"></a>O podsítě</span><span class="sxs-lookup"><span data-stu-id="5a338-138"><a name="subnets"></a>About subnets</span></span>

<span data-ttu-id="5a338-139">Při vytváření připojení, musíte zvážit vaší rozsahy podsítě.</span><span class="sxs-lookup"><span data-stu-id="5a338-139">When you are creating connections, you must consider your subnet ranges.</span></span> <span data-ttu-id="5a338-140">Nemůže mít překrývající se rozsahy adres podsítě.</span><span class="sxs-lookup"><span data-stu-id="5a338-140">You cannot have overlapping subnet address ranges.</span></span> <span data-ttu-id="5a338-141">Překrývající se podsítí je, když jeden virtuální síti nebo na místní umístění obsahuje hello, který obsahuje stejný adresní prostor, který hello jiného umístění.</span><span class="sxs-lookup"><span data-stu-id="5a338-141">An overlapping subnet is when one virtual network or on-premises location contains hello same address space that hello other location contains.</span></span> <span data-ttu-id="5a338-142">To znamená, třeba technici vaší sítě pro vaše místní sítě toocarve se rozsah adres pro toouse můžete pro Azure na IP adresování místa nebo podsítě.</span><span class="sxs-lookup"><span data-stu-id="5a338-142">This means that you need your network engineers for your local on-premises networks toocarve out a range for you toouse for your Azure IP addressing space/subnets.</span></span> <span data-ttu-id="5a338-143">Je nutné adresní prostor, který se nepoužívá v místní síti hello.</span><span class="sxs-lookup"><span data-stu-id="5a338-143">You need address space that is not being used on hello local on-premises network.</span></span>

<span data-ttu-id="5a338-144">Zamezení překrývající se podsítí je také důležité, když pracujete s připojení VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="5a338-144">Avoiding overlapping subnets is also important when you are working with VNet-to-VNet connections.</span></span> <span data-ttu-id="5a338-145">Pokud podsítě překrývajících se a IP adresu v odesílání hello i cílové virtuální sítě existuje, připojení VNet-to-VNet se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="5a338-145">If your subnets overlap and an IP address exists in both hello sending and destination VNets, VNet-to-VNet connections fail.</span></span> <span data-ttu-id="5a338-146">Azure nelze směrovat hello data toohello jiné virtuální sítě, protože hello cílová adresa je součástí hello odesílání virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5a338-146">Azure can't route hello data toohello other VNet because hello destination address is part of hello sending VNet.</span></span>

<span data-ttu-id="5a338-147">Brány sítě VPN vyžaduje konkrétní podsíť s názvem podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="5a338-147">VPN Gateways require a specific subnet called a gateway subnet.</span></span> <span data-ttu-id="5a338-148">Všechny podsítě brány musí mít název GatewaySubnet toowork správně.</span><span class="sxs-lookup"><span data-stu-id="5a338-148">All gateway subnets must be named GatewaySubnet toowork properly.</span></span> <span data-ttu-id="5a338-149">Ujistěte se, není tooname podsítě brány na jiný název a nenasazujte virtuální počítače ani cokoli jiného toohello podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="5a338-149">Be sure not tooname your gateway subnet a different name, and don't deploy VMs or anything else toohello gateway subnet.</span></span> <span data-ttu-id="5a338-150">V tématu [podsítě brány](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span><span class="sxs-lookup"><span data-stu-id="5a338-150">See [Gateway Subnets](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span></span>

#### <span data-ttu-id="5a338-151"><a name="local"></a>O brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="5a338-151"><a name="local"></a>About local network gateways</span></span>

<span data-ttu-id="5a338-152">Hello brány místní sítě obvykle odkazuje tooyour místní umístění.</span><span class="sxs-lookup"><span data-stu-id="5a338-152">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="5a338-153">V modelu nasazení classic hello je brána místní sítě hello odkazované tooas místní síťové lokality.</span><span class="sxs-lookup"><span data-stu-id="5a338-153">In hello classic deployment model, hello local network gateway is referred tooas a Local Network Site.</span></span> <span data-ttu-id="5a338-154">Pokud budete konfigurovat bránu místní sítě, zadejte jeho název, zadejte hello veřejná IP adresa zařízení VPN místní hello a zadejte hello předpony, které se nacházejí v umístění místní hello.</span><span class="sxs-lookup"><span data-stu-id="5a338-154">When you configure a local network gateway, you give it a name, specify hello public IP address of hello on-premises VPN device, and specify hello address prefixes that are in hello on-premises location.</span></span> <span data-ttu-id="5a338-155">Azure zjistí hello předpony cílových adres pro síťový provoz, zajímají hello konfigurace, který jste zadali pro bránu místní sítě hello a směruje pakety odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5a338-155">Azure looks at hello destination address prefixes for network traffic, consults hello configuration that you have specified for hello local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="5a338-156">Předpony adres hello můžete upravit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5a338-156">You can modify hello address prefixes as needed.</span></span> <span data-ttu-id="5a338-157">Další informace najdete v tématu [brány místní sítě](vpn-gateway-about-vpn-gateway-settings.md#lng).</span><span class="sxs-lookup"><span data-stu-id="5a338-157">For more information, see [Local network gateways](vpn-gateway-about-vpn-gateway-settings.md#lng).</span></span>

#### <span data-ttu-id="5a338-158"><a name="gwtype"></a>O typech brány</span><span class="sxs-lookup"><span data-stu-id="5a338-158"><a name="gwtype"></a>About gateway types</span></span>

<span data-ttu-id="5a338-159">Výběr typu hello správné brány pro vaše topologie je velmi důležité.</span><span class="sxs-lookup"><span data-stu-id="5a338-159">Selecting hello correct gateway type for your topology is critical.</span></span> <span data-ttu-id="5a338-160">Pokud vyberete hello chybný typ, brána nebude fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="5a338-160">If you select hello wrong type, your gateway won't work properly.</span></span> <span data-ttu-id="5a338-161">Typ brány Hello Určuje, jak samotné bráně hello připojí a je požadované nastavení pro model nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="5a338-161">hello gateway type specifies how hello gateway itself connects and is a required configuration setting for hello Resource Manager deployment model.</span></span>

<span data-ttu-id="5a338-162">Hello brány typy jsou:</span><span class="sxs-lookup"><span data-stu-id="5a338-162">hello gateway types are:</span></span>

* <span data-ttu-id="5a338-163">Vpn</span><span class="sxs-lookup"><span data-stu-id="5a338-163">Vpn</span></span>
* <span data-ttu-id="5a338-164">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5a338-164">ExpressRoute</span></span>

#### <span data-ttu-id="5a338-165"><a name="connectiontype"></a>O typech připojení</span><span class="sxs-lookup"><span data-stu-id="5a338-165"><a name="connectiontype"></a>About connection types</span></span>

<span data-ttu-id="5a338-166">Každá konfigurace vyžaduje určitý typ připojení.</span><span class="sxs-lookup"><span data-stu-id="5a338-166">Each configuration requires a specific connection type.</span></span> <span data-ttu-id="5a338-167">Hello typy připojení jsou:</span><span class="sxs-lookup"><span data-stu-id="5a338-167">hello connection types are:</span></span>

* <span data-ttu-id="5a338-168">Protokol IPsec</span><span class="sxs-lookup"><span data-stu-id="5a338-168">IPsec</span></span>
* <span data-ttu-id="5a338-169">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="5a338-169">Vnet2Vnet</span></span>
* <span data-ttu-id="5a338-170">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="5a338-170">ExpressRoute</span></span>
* <span data-ttu-id="5a338-171">VPNClient</span><span class="sxs-lookup"><span data-stu-id="5a338-171">VPNClient</span></span>

#### <span data-ttu-id="5a338-172"><a name="vpntype"></a>O typy sítě VPN</span><span class="sxs-lookup"><span data-stu-id="5a338-172"><a name="vpntype"></a>About VPN types</span></span>

<span data-ttu-id="5a338-173">Každá konfigurace vyžaduje určitý typ sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="5a338-173">Each configuration requires a specific VPN type.</span></span> <span data-ttu-id="5a338-174">Pokud kombinujete dvě konfigurace, jako je například vytváření připojení Site-to-Site a toohello připojení Point-to-Site stejné virtuální síti, musíte použít typ sítě VPN, který splňuje požadavky obou připojení.</span><span class="sxs-lookup"><span data-stu-id="5a338-174">If you are combining two configurations, such as creating a Site-to-Site connection and a Point-to-Site connection toohello same VNet, you must use a VPN type that satisfies both connection requirements.</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="5a338-175">Hello následující tabulky popisují typ sítě VPN hello jako mapuje tooeach konfigurace připojení.</span><span class="sxs-lookup"><span data-stu-id="5a338-175">hello following tables show hello VPN type as it maps tooeach connection configuration.</span></span> <span data-ttu-id="5a338-176">Ujistěte se, zda text hello typ sítě VPN pro konfiguraci brány odpovídá hello, které chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="5a338-176">Make sure hello VPN type for your gateway matches hello configuration that you want toocreate.</span></span> 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <span data-ttu-id="5a338-177"><a name="devices"></a>Zařízení VPN pro připojení Site-to-Site</span><span class="sxs-lookup"><span data-stu-id="5a338-177"><a name="devices"></a>VPN devices for Site-to-Site connections</span></span>

<span data-ttu-id="5a338-178">připojení tooconfigure Site-to-Site, bez ohledu na modelu nasazení, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a338-178">tooconfigure a Site-to-Site connection, regardless of deployment model, you need hello following items:</span></span>

* <span data-ttu-id="5a338-179">Zařízení VPN, který je kompatibilní s Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="5a338-179">A VPN device that is compatible with Azure VPN gateways</span></span>
* <span data-ttu-id="5a338-180">Veřejnou adresu IPv4 IP, který není za zařízení NAT</span><span class="sxs-lookup"><span data-stu-id="5a338-180">A public-facing IPv4 IP address that is not behind a NAT</span></span>

<span data-ttu-id="5a338-181">Budete potřebovat prostředí toohave konfiguraci zařízení VPN, nebo požádat uživatele, který může nakonfigurovat zařízení hello za vás.</span><span class="sxs-lookup"><span data-stu-id="5a338-181">You need toohave experience configuring your VPN device, or have someone that can configure hello device for you.</span></span>

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <span data-ttu-id="5a338-182"><a name="forcedtunnel"></a>Vezměte v úvahu vynucené tunelového propojení směrování</span><span class="sxs-lookup"><span data-stu-id="5a338-182"><a name="forcedtunnel"></a>Consider forced tunnel routing</span></span>

<span data-ttu-id="5a338-183">Pro většinu konfiguraci můžete konfigurovat vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="5a338-183">For most configurations, you can configure forced tunneling.</span></span> <span data-ttu-id="5a338-184">Vynutit tunelové propojení umožňuje přesměrování nebo "Vynutit" všechny internetový provoz back tooyour místní umístění prostřednictvím tunelu Site-to-Site VPN pro kontrolu a auditování.</span><span class="sxs-lookup"><span data-stu-id="5a338-184">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="5a338-185">Požadavek kritické zabezpečení pro většinu organizace IT zásad.</span><span class="sxs-lookup"><span data-stu-id="5a338-185">This is a critical security requirement for most enterprise IT policies.</span></span> 

<span data-ttu-id="5a338-186">Bez vynucené tunelování, internetový provoz z virtuálních počítačů v Azure bude vždy procházení od Azure síťové infrastruktury přímo na Internetu, toohello bez tooallow možnost hello je provoz hello tooinspect nebo kontrola.</span><span class="sxs-lookup"><span data-stu-id="5a338-186">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="5a338-187">Neoprávněný přístup k Internetu může potenciálně vést tooinformation zpřístupnění nebo jiné typy narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5a338-187">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

<span data-ttu-id="5a338-188">V obou modelech nasazení a pomocí jiných nástrojů se dá konfigurovat vynucené tunelové připojení.</span><span class="sxs-lookup"><span data-stu-id="5a338-188">A forced tunneling connection can be configured in both deployment models and by using different tools.</span></span> <span data-ttu-id="5a338-189">Další informace najdete v tématu [konfigurace vynuceného tunelování](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="5a338-189">For more information, see [Configure forced tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>

<span data-ttu-id="5a338-190">**Vynutit tunelové diagram**</span><span class="sxs-lookup"><span data-stu-id="5a338-190">**Forced tunneling diagram**</span></span>

![Vynutit tunelové diagram služby Azure VPN Gateway](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a><span data-ttu-id="5a338-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a338-192">Next steps</span></span>

<span data-ttu-id="5a338-193">V tématu hello [VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md) a [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) články pro další informace o toohelp jste s návrhu.</span><span class="sxs-lookup"><span data-stu-id="5a338-193">See hello [VPN Gateway FAQ](vpn-gateway-vpn-faq.md) and [About VPN Gateway](vpn-gateway-about-vpngateways.md) articles for more information toohelp you with your design.</span></span>

<span data-ttu-id="5a338-194">Další informace o nastavení konkrétní brány najdete v tématu [o nastavení brány sítě VPN](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="5a338-194">For more information about specific gateway settings, see [About VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>