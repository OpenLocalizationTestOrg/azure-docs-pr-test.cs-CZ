---
title: "Ukázky konfigurace směrovače zákazníka aaaExpressRoute | Microsoft Docs"
description: "Tato stránka obsahuje ukázky konfigurace směrovače pro směrovače Cisco a Juniper."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 564826bc-017a-4683-a385-37c9fa814948
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: 5c91f24e6082e01c3e8df91b4fcfda46a6c29fa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="router-configuration-samples-tooset-up-and-manage-routing"></a><span data-ttu-id="cba5f-103">Konfigurace směrovače ukázky tooset nahoru a spravovat směrování</span><span class="sxs-lookup"><span data-stu-id="cba5f-103">Router configuration samples tooset up and manage routing</span></span>
<span data-ttu-id="cba5f-104">Tato stránka obsahuje rozhraní a směrování ukázky konfigurace pro Cisco IOS-XE a Juniper MX směrovače řady.</span><span class="sxs-lookup"><span data-stu-id="cba5f-104">This page provides interface and routing configuration samples for Cisco IOS-XE and Juniper MX series routers.</span></span> <span data-ttu-id="cba5f-105">Tyto jsou ukázky určený toobe pouze pokyny a nesmí se používat, protože je.</span><span class="sxs-lookup"><span data-stu-id="cba5f-105">These are intended toobe samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="cba5f-106">Můžete pracovat s vaší toocome dodavatele s odpovídající konfigurací pro vaši síť.</span><span class="sxs-lookup"><span data-stu-id="cba5f-106">You can work with your vendor toocome up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="cba5f-107">Ukázky na této stránce jsou toobe určený výhradně pro pokyny.</span><span class="sxs-lookup"><span data-stu-id="cba5f-107">Samples in this page are intended toobe purely for guidance.</span></span> <span data-ttu-id="cba5f-108">Musíte pracovat se tým prodeje / technické od dodavatele a vaší sítě team toocome až s odpovídající konfigurací toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="cba5f-108">You must work with your vendor's sales / technical team and your networking team toocome up with appropriate configurations toomeet your needs.</span></span> <span data-ttu-id="cba5f-109">Microsoft nebude podporovat problémy související s tooconfigurations uvedené na této stránce.</span><span class="sxs-lookup"><span data-stu-id="cba5f-109">Microsoft will not support issues related tooconfigurations listed in this page.</span></span> <span data-ttu-id="cba5f-110">Pro problémy podpory, bude nutné kontaktovat dodavatele zařízení.</span><span class="sxs-lookup"><span data-stu-id="cba5f-110">You must contact your device vendor for support issues.</span></span>
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a><span data-ttu-id="cba5f-111">Nastavení jednotek MTU a MSS protokolu TCP na rozhraní směrovače</span><span class="sxs-lookup"><span data-stu-id="cba5f-111">MTU and TCP MSS settings on router interfaces</span></span>
* <span data-ttu-id="cba5f-112">Hello MTU pro rozhraní ExpressRoute hello je 1500, což je typický výchozí hello MTU pro rozhraní sítě Ethernet na směrovač.</span><span class="sxs-lookup"><span data-stu-id="cba5f-112">hello MTU for hello ExpressRoute interface is 1500, which is hello typical default MTU for an Ethernet interface on a router.</span></span> <span data-ttu-id="cba5f-113">Pokud ve výchozím nastavení má směrovač jiné MTU, je bez nutnosti toospecify hodnotu na rozhraní směrovače hello.</span><span class="sxs-lookup"><span data-stu-id="cba5f-113">Unless your router has a different MTU by default, there is no need toospecify a value on hello router interface.</span></span>
* <span data-ttu-id="cba5f-114">Na rozdíl od služby Azure VPN Gateway hello MSS protokolu TCP pro okruh ExpressRoute není nutné toobe zadán.</span><span class="sxs-lookup"><span data-stu-id="cba5f-114">Unlike an Azure VPN Gateway, hello TCP MSS for an ExpressRoute circuit does not need toobe specified.</span></span>

<span data-ttu-id="cba5f-115">Následující ukázky konfigurace směrovače použít tooall partnerských vztahů.</span><span class="sxs-lookup"><span data-stu-id="cba5f-115">Router configuration samples below apply tooall peerings.</span></span> <span data-ttu-id="cba5f-116">Zkontrolujte [partnerských vztahů ExpressRoute](expressroute-circuit-peerings.md) a [požadavky na směrování služby ExpressRoute](expressroute-routing.md) Další informace o směrování.</span><span class="sxs-lookup"><span data-stu-id="cba5f-116">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute routing requirements](expressroute-routing.md) for more details on routing.</span></span>


## <a name="cisco-ios-xe-based-routers"></a><span data-ttu-id="cba5f-117">Cisco IOS-XE na základě směrovače</span><span class="sxs-lookup"><span data-stu-id="cba5f-117">Cisco IOS-XE based routers</span></span>
<span data-ttu-id="cba5f-118">Ukázky Hello v této části se vztahují na všechny směrovač se systémem IOS XE operačních systémů hello.</span><span class="sxs-lookup"><span data-stu-id="cba5f-118">hello samples in this section apply for any router running hello IOS-XE OS family.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="cba5f-119">1. Konfigurace rozhraní a dílčí rozhraní</span><span class="sxs-lookup"><span data-stu-id="cba5f-119">1. Configuring interfaces and sub-interfaces</span></span>
<span data-ttu-id="cba5f-120">Budete potřebovat dílčí rozhraní na partnerský vztah v každé směrovače můžete připojit tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="cba5f-120">You will require a sub interface per peering in every router you connect tooMicrosoft.</span></span> <span data-ttu-id="cba5f-121">Sub – rozhraní možné identifikovat pomocí ID sítě VLAN nebo pár skládaný ID sítě VLAN a IP adresu.</span><span class="sxs-lookup"><span data-stu-id="cba5f-121">A sub interface can be identified with a VLAN ID or a stacked pair of VLAN IDs and an IP address.</span></span>

<span data-ttu-id="cba5f-122">**Definice rozhraní Dot1Q**</span><span class="sxs-lookup"><span data-stu-id="cba5f-122">**Dot1Q interface definition**</span></span>

<span data-ttu-id="cba5f-123">Tato ukázka poskytuje hello Definice dílčí rozhraní pro dílčí rozhraní s jeden ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="cba5f-123">This sample provides hello sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="cba5f-124">Hello ID sítě VLAN je jedinečný pro každého partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="cba5f-124">hello VLAN ID is unique per peering.</span></span> <span data-ttu-id="cba5f-125">poslední oktet Hello vaší adresy IPv4, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="cba5f-125">hello last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

<span data-ttu-id="cba5f-126">**Definice rozhraní QinQ**</span><span class="sxs-lookup"><span data-stu-id="cba5f-126">**QinQ interface definition**</span></span>

<span data-ttu-id="cba5f-127">Tato ukázka obsahuje hello Definice dílčí rozhraní pro dílčí rozhraní se dva identifikátory ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="cba5f-127">This sample provides hello sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="cba5f-128">Dobrý den, který zůstane vnější ID sítě VLAN (s-tag), pokud se používá stejné hello napříč všech partnerských vztahů hello.</span><span class="sxs-lookup"><span data-stu-id="cba5f-128">hello outer VLAN ID (s-tag), if used remains hello same across all hello peerings.</span></span> <span data-ttu-id="cba5f-129">vnitřní Hello ID sítě VLAN (c-tag) je jedinečný pro každého partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="cba5f-129">hello inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="cba5f-130">poslední oktet Hello vaší adresy IPv4, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="cba5f-130">hello last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="cba5f-131">2. Nastavení relace eBGP</span><span class="sxs-lookup"><span data-stu-id="cba5f-131">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="cba5f-132">Musíte nastavit relace protokolu BGP se společností Microsoft pro každý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="cba5f-132">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="cba5f-133">Následující ukázka Hello umožňuje toosetup relaci protokolu BGP se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cba5f-133">hello sample below enables you toosetup a BGP session with Microsoft.</span></span> <span data-ttu-id="cba5f-134">Pokud hello IPv4 adresu, která jste použili pro vaše rozhraní dílčí a.b.c.d, hello IP adresu BGP sousedním hello (Microsoft) bude a.b.c.d+1.</span><span class="sxs-lookup"><span data-stu-id="cba5f-134">If hello IPv4 address you used for your sub interface was a.b.c.d, hello IP address of hello BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="cba5f-135">poslední oktet Hello sousedním BGP hello adresy IPv4, bude vždy sudé číslo.</span><span class="sxs-lookup"><span data-stu-id="cba5f-135">hello last octet of hello BGP neighbor's IPv4 address will always be an even number.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a><span data-ttu-id="cba5f-136">3. Nastavení toobe předpon inzerovaných během relace protokolu BGP hello</span><span class="sxs-lookup"><span data-stu-id="cba5f-136">3. Setting up prefixes toobe advertised over hello BGP session</span></span>
<span data-ttu-id="cba5f-137">Můžete nakonfigurovat tooMicrosoft vyberte předpony tooadvertise vašeho směrovače.</span><span class="sxs-lookup"><span data-stu-id="cba5f-137">You can configure your router tooadvertise select prefixes tooMicrosoft.</span></span> <span data-ttu-id="cba5f-138">Můžete toho dosáhnout pomocí hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="cba5f-138">You can do so using hello sample below.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a><span data-ttu-id="cba5f-139">4. Mapuje trasy</span><span class="sxs-lookup"><span data-stu-id="cba5f-139">4. Route maps</span></span>
<span data-ttu-id="cba5f-140">Můžete použít mapy trasy a předponu uvádí toofilter předpony rozšíří do vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="cba5f-140">You can use route-maps and prefix lists toofilter prefixes propagated into your network.</span></span> <span data-ttu-id="cba5f-141">Můžete použít ukázkové hello níže tooaccomplish hello úloh.</span><span class="sxs-lookup"><span data-stu-id="cba5f-141">You can use hello sample below tooaccomplish hello task.</span></span> <span data-ttu-id="cba5f-142">Ujistěte se, že máte příslušné předpony zobrazí instalační program.</span><span class="sxs-lookup"><span data-stu-id="cba5f-142">Ensure that you have appropriate prefix lists setup.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a><span data-ttu-id="cba5f-143">Směrovače Juniper MX řady</span><span class="sxs-lookup"><span data-stu-id="cba5f-143">Juniper MX series routers</span></span>
<span data-ttu-id="cba5f-144">Ukázky Hello v této části se vztahují na veškeré směrovače Juniper MX řady.</span><span class="sxs-lookup"><span data-stu-id="cba5f-144">hello samples in this section apply for any Juniper MX series routers.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="cba5f-145">1. Konfigurace rozhraní a dílčí rozhraní</span><span class="sxs-lookup"><span data-stu-id="cba5f-145">1. Configuring interfaces and sub-interfaces</span></span>

<span data-ttu-id="cba5f-146">**Definice rozhraní Dot1Q**</span><span class="sxs-lookup"><span data-stu-id="cba5f-146">**Dot1Q interface definition**</span></span>

<span data-ttu-id="cba5f-147">Tato ukázka poskytuje hello Definice dílčí rozhraní pro dílčí rozhraní s jeden ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="cba5f-147">This sample provides hello sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="cba5f-148">Hello ID sítě VLAN je jedinečný pro každého partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="cba5f-148">hello VLAN ID is unique per peering.</span></span> <span data-ttu-id="cba5f-149">poslední oktet Hello vaší adresy IPv4, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="cba5f-149">hello last octet of your IPv4 address will always be an odd number.</span></span>

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


<span data-ttu-id="cba5f-150">**Definice rozhraní QinQ**</span><span class="sxs-lookup"><span data-stu-id="cba5f-150">**QinQ interface definition**</span></span>

<span data-ttu-id="cba5f-151">Tato ukázka obsahuje hello Definice dílčí rozhraní pro dílčí rozhraní se dva identifikátory ID sítě VLAN.</span><span class="sxs-lookup"><span data-stu-id="cba5f-151">This sample provides hello sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="cba5f-152">Dobrý den, který zůstane vnější ID sítě VLAN (s-tag), pokud se používá stejné hello napříč všech partnerských vztahů hello.</span><span class="sxs-lookup"><span data-stu-id="cba5f-152">hello outer VLAN ID (s-tag), if used remains hello same across all hello peerings.</span></span> <span data-ttu-id="cba5f-153">vnitřní Hello ID sítě VLAN (c-tag) je jedinečný pro každého partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="cba5f-153">hello inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="cba5f-154">poslední oktet Hello vaší adresy IPv4, bude vždy lichý počet.</span><span class="sxs-lookup"><span data-stu-id="cba5f-154">hello last octet of your IPv4 address will always be an odd number.</span></span>

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="cba5f-155">2. Nastavení relace eBGP</span><span class="sxs-lookup"><span data-stu-id="cba5f-155">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="cba5f-156">Musíte nastavit relace protokolu BGP se společností Microsoft pro každý partnerský vztah.</span><span class="sxs-lookup"><span data-stu-id="cba5f-156">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="cba5f-157">Následující ukázka Hello umožňuje toosetup relaci protokolu BGP se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cba5f-157">hello sample below enables you toosetup a BGP session with Microsoft.</span></span> <span data-ttu-id="cba5f-158">Pokud hello IPv4 adresu, která jste použili pro vaše rozhraní dílčí a.b.c.d, hello IP adresu BGP sousedním hello (Microsoft) bude a.b.c.d+1.</span><span class="sxs-lookup"><span data-stu-id="cba5f-158">If hello IPv4 address you used for your sub interface was a.b.c.d, hello IP address of hello BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="cba5f-159">poslední oktet Hello sousedním BGP hello adresy IPv4, bude vždy sudé číslo.</span><span class="sxs-lookup"><span data-stu-id="cba5f-159">hello last octet of hello BGP neighbor's IPv4 address will always be an even number.</span></span>

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a><span data-ttu-id="cba5f-160">3. Nastavení toobe předpon inzerovaných během relace protokolu BGP hello</span><span class="sxs-lookup"><span data-stu-id="cba5f-160">3. Setting up prefixes toobe advertised over hello BGP session</span></span>
<span data-ttu-id="cba5f-161">Můžete nakonfigurovat tooMicrosoft vyberte předpony tooadvertise vašeho směrovače.</span><span class="sxs-lookup"><span data-stu-id="cba5f-161">You can configure your router tooadvertise select prefixes tooMicrosoft.</span></span> <span data-ttu-id="cba5f-162">Můžete toho dosáhnout pomocí hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="cba5f-162">You can do so using hello sample below.</span></span>

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a><span data-ttu-id="cba5f-163">4. Mapuje trasy</span><span class="sxs-lookup"><span data-stu-id="cba5f-163">4. Route maps</span></span>
<span data-ttu-id="cba5f-164">Můžete použít mapy trasy a předponu uvádí toofilter předpony rozšíří do vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="cba5f-164">You can use route-maps and prefix lists toofilter prefixes propagated into your network.</span></span> <span data-ttu-id="cba5f-165">Můžete použít ukázkové hello níže tooaccomplish hello úloh.</span><span class="sxs-lookup"><span data-stu-id="cba5f-165">You can use hello sample below tooaccomplish hello task.</span></span> <span data-ttu-id="cba5f-166">Ujistěte se, že máte příslušné předpony zobrazí instalační program.</span><span class="sxs-lookup"><span data-stu-id="cba5f-166">Ensure that you have appropriate prefix lists setup.</span></span>

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a><span data-ttu-id="cba5f-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cba5f-167">Next Steps</span></span>
<span data-ttu-id="cba5f-168">V tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cba5f-168">See hello [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>

